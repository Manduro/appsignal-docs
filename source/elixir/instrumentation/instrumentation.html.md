---
title: "Custom instrumentation for Elixir"
---

In order to find out what specific pieces of code are causing performance
problems it's useful to add custom instrumentation to your application. This
allows us to create better breakdowns of which code runs slowest and what type
of action the most amount of time was spent on.

Custom instrumentation is possible in two ways: using function decorators and
instrumentation helper functions. The function decorators are easiest to use,
but are less flexible than the instrumentation helper functions.

This short guide will help you set up custom instrumentation. More details on
the usage of certain helpers can be found in the Hex docs for the [AppSignal
package](https://hexdocs.pm/appsignal/).

-> **Note**: Make sure you've [integrated
   AppSignal](/elixir/instrumentation/integrating-appsignal.html) before adding
   custom instrumentation to your application if it's not automatically
   integrated by one of our supported
   [integrations](/elixir/integrations/index.html).

-> **Note**: This page only describes how to add performance instrumentation to
   your code. To track errors please read our
   [exception handling](/elixir/instrumentation/exception-handling.html) guide.

## `Appsignal.instrument/2-3`

The `instrument/2` function is used to add instrumentation by wrapping a piece
of code in a span. A span eventually becomes a sample, or an event in another
span's sample in AppSignal.

### Add spans to traces

In the following example we have a Phoenix controller with an `index/2`
function which calls a slow function. The `slow` function is instrumented using
the `Appsignal.instrument/2` function which records it as a separate event in
this Phoenix request. It will show up on AppSignal.com in the event timeline of
this sample to provide more insight in where the most time was spent during the
request.

```elixir
defmodule AppsignalPhoenixExampleWeb.PageController do
  use AppsignalPhoenixExampleWeb, :controller

  def index(conn, _params) do
    slow()
    render(conn, "index.html")
  end

  defp slow do
    Appsignal.instrument("slow", fn ->
      :timer.sleep(1000)
    end)
  end
end
```

Here, we wrap our function's contents with a call to `Appsignal.instrument3`,
and we pass `"slow"` as the name for the event.

### Starting new traces

In the Phoenix example an AppSignal trace has already been started, thanks to
the first-party support for Phoenix in the AppSignal package. Not all
frameworks and packages are currently supported directly and automatically
start traces. The same is true for your own pure Elixir applications.

Like when adding spans to already-instrumented traces, a new root span is
created using the `Appsignal.instrument/2` function:

```elixir
defmodule AppsignalElixirExample do
  def example_fn do
    Appsignal.instrument("instrument", fn ->
      :timer.sleep(500)
    end)
  end
end
```

This example creates a sample named "instrument" in the "background" namespace
in AppSignal. The name will also be used as the category name for the main
span. To use a different category name, use `instrument/3` instead:

```elixir
defmodule AppsignalElixirExample do
  def example_fn do
    Appsignal.instrument("instrument", "call.instrument", fn ->
      :timer.sleep(500)
    end)
  end
end
```

When passing a function that takes an argument, the `instrument/2-3` function calls it with the opened span to allow further customization. See the docs on hex.pm for [all available Span functions](https://hexdocs.pm/appsignal/Appsignal.Span.html).

```elixir
defmodule AppsignalElixirExample do
  def example_fn do
    Appsignal.instrument("prepare_query.sql", "Build complicated SQL query", fn span ->
      # Example of building a complex SQL query in the instrument block and performing it
      query = "SOME complicate SQL query"
      # The body is only set here because it's the value being calculated and instrumented in this function
      Appsignal.Span.set_sql(span, query)
      Database.perform_query(query)
    end)
  end
end
```

###^helper Exception handling

To report errors using custom instrumentation please read more in our
[exception handling guide](/elixir/instrumentation/exception-handling.html).

## Function decorators

Using the `Appsignal.Instrumentation.Decorators` decorator module, it's
possible add custom instrumentation to your Elixir applications without
changing the functions' contents.

###^decorator Transaction events

In the following example we have a Phoenix controller with an `index/2`
function which calls a slow function. The `slow` function is instrumented using
the AppSignal `transaction_event` decorator which records it as a separate
event in this Phoenix request. It will show up on AppSignal.com in the event
timeline of this transaction sample to provide more insight in where the most
time was spent during the request.

```elixir
# Phoenix controller example
defmodule PhoenixExample.PageController do
  use PhoenixExample.Web, :controller
  # Include this
  use Appsignal.Instrumentation.Decorators

  def index(conn, _params) do
    slow()
    render conn, "index.html"
  end

  # Decorate this function to add custom instrumentation
  @decorate transaction_event()
  defp slow do
    :timer.sleep(1000)
  end
end
```

If you want to group certain events together under the same event group (other
group are `phoenix_controller`, `phoenix_render`, `ecto`, etc.) you can also
supply a group name to the `transaction_event` decorator.

```elixir
@decorate transaction_event("github_api")
defp get_data_from_github do
  # Third-party API call
end
```

This will create an event `get_data_from_github.github_api` in the event
timeline. For more information on how event names are used, please read
our [event naming guidelines](/api/event-names.html).

###^decorator Transactions

In the Phoenix example an AppSignal transaction has already been started,
thanks to the first-party support for Phoenix in the AppSignal package. Not all
frameworks and packages are currently supported directly and automatically
start transactions. The same is true for your own pure Elixir applications.

In order to track `transaction_event` decorators we will need to start an
AppSignal transaction beforehand. We can start a transaction with the
`transaction` function decorator.

```elixir
# Pure Elixir example
defmodule FunctionDecoratorsExample do
  # Include this
  use Appsignal.Instrumentation.Decorators

  # No transaction is started beforehand like in Phoenix, so we need to start
  # it ourselves.
  @decorate transaction()
  def call do
    slow()
    # ...
  end

  # Decorate this function to add custom instrumentation
  @decorate transaction_event()
  defp slow do
    :timer.sleep(1000)
  end
end
```

**Note**: When using pure Elixir applications, make sure that the AppSignal
application is started before you start a transaction. For more information,
see how to
[integrate AppSignal](/elixir/instrumentation/integrating-appsignal.html).

###^decorator Namespaces

In order to differentiate between HTTP requests and background jobs we can pass
a namespace to the transaction once we start it.

The following two namespaces are official namespaces supported by AppSignal.

- `http_request` - the default - is called the "web" namespace
- `background_job` - creates the "background" namespace

```elixir
defmodule FunctionDecoratorsExample do
  # Include this
  use Appsignal.Instrumentation.Decorators

  # No namespace argument defaults to `:http_request`
  @decorate transaction()
  def web_function do
    # do stuff
  end

  # The "background" namespace
  @decorate transaction(:background_job)
  def background_function do
    # do stuff
  end
end
```

For more information about what namespaces are, please see our
[namespaces](/application/namespaces.html) documentation.

###^decorator Custom namespaces

You can also create your own namespaces to track transactions in a separate
part of your application such as an administration panel. This will group all
the transactions with this namespace in a separate section on AppSignal.com so
that slow admin controllers don't interfere with the averages of your
application's speed.

```elixir
@decorate transaction(:admin)
def some_function do
  # do stuff
end
```

###^decorator Phoenix channels

There is a custom function decorator for Phoenix channels. This decorator is
meant to be put before the `handle_in/3` function of a `Phoenix.Channel`
module.

```elixir
defmodule FunctionDecoratorsExample.MyChannel do
  # Include this
  use Appsignal.Instrumentation.Decorators

  # Add this channel function decorator
  @decorate channel_action()
  def handle_in("ping", _payload, socket) do
    # your code here..
  end
end
```

Channel events will be displayed under the "background" namespace, showing the
channel module and the action argument that it's used on.

## Manually creating and closing spans

In some cases it can be useful to manually handle spans. You can use the
span API to instrument tasks for example:

```elixir
def index(conn, _params) do
  current = Appsignal.Tracer.current_span()
  fn ->
    span = "http_request"
    |> Appsignal.Tracer.create_span(current)
    |> Appsignal.Span.set_name("name")
    |> Appsignal.Span.set_attribute("appsignal:category", "category")
    :timer.sleep(1000)
    Appsignal.Tracer.close_span(span)
  end
  |> Task.async()
  |> Task.await()
  render(conn, "index.html")
end
```
