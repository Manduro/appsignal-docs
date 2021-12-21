---
title: "Tracing"
---

AppSignal provides many ways to provide more insights into your application - by adding more instrumentation or tagging the data that appears in the UI at AppSignal.com. All of the information that we collect from your application forms part of a broader **trace**, a visual representation of the flow of data through your application.

**Traces** are made of one or many `Span`s. A Trace can be thought of as a _directed acyclic graph (DAG)_ of `Span`s:

![Trace diagram](/assets/images/abstract-trace.png)

* [The Tracer object](tracer.html)
* [Creating and using a Span](span.html)
* [Exception handling](exception-handling.html)
* [Async tracing and Scopes](scopes.html)

## Contact us

Don't hesitate to [contact us](mailto:support@appsignal.com) if you run into
any issues while implementing tracing. We're here to help.
