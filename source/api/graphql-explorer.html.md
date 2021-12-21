---
title: "GraphQL Explorer"
---
Use this GraphQL explorer to play around with AppSignal's GraphQL API. The information you see within AppSignal UI is fetched using this API.

## Explorer

<div class="c-form">
  <label for="token" class="c-form__label">Enter your personal API <a href="https://appsignal.com/users/edit" target="_blank">token</a> </label><br>
  <input type="text" id="token" name="token" class="c-form__input">
</div>
<div id="graphiql" style="height: 100vh;margin-bottom:40px">Loading..</div>

## Examples
Here are some of the query examples to get you started.
### Fetch deployments
#### Query
```graphql
query MarkersIndexQuery($appId: String!, $limit: Int, $offset: Int, $start: DateTime, $end: DateTime) {
  app(id: $appId) {
    id
    deployMarkers(limit: $limit, offset: $offset, start: $start, end: $end) {
      id
      createdAt
      shortRevision
      revision
      gitCompareUrl
      user
      liveForInWords
      liveFor
      exceptionCount
      exceptionRate
      __typename
    }
    __typename
  }
}
```
#### Query variables
```graphql
{
  "appId": "YOUR-APP-ID",
  "limit": 25
}
```

###  Fetch incidents by deployments
#### Query
```graphql
query ExceptionIncidentsQuery($appId: String!, $namespaces: [String], $markerId: String, $limit: Int, $offset: Int, $state: IncidentStateEnum, $order: IncidentOrderEnum) {
  app(id: $appId) {
    id
    exceptionIncidents(namespaces: $namespaces, marker: $markerId, limit: $limit, state: $state, offset: $offset, order: $order) {
      ...ExceptionIncidentRow
      __typename
    }
    __typename
  }
}

fragment ExceptionIncidentRow on ExceptionIncident {
  id
  number
  count
  perMarkerCount(marker: $markerId)
  lastOccurredAt
  actionNames
  exceptionName
  state
  namespace
  firstBacktraceLine
  errorGroupingStrategy
  severity
}
```
#### Query variables
```graphql
{
  "appId": "YOUR-APP-ID",
  "markerId": "YOUR-DEPLOY-MARKER-ID",
  "limit": 200
}
```

###  Search incidents by tag
#### Query
```graphql
uery Search(
    $organizationSlug: String!
    $query: String
    $namespace: String
    $sampleType: SampleTypeEnum
  ) {
    organization(slug: $organizationSlug) {
      search(
        query: $query
        namespace: $namespace
        sampleType: $sampleType
      ) {
        ... on ExceptionSample {
          id
          time
          action
          namespace
          overview {
            key
            value
          }
          exception {
            name
            message
          }
          incident {
            ... on ExceptionIncident {
              number
            }
          }
          app {
            name
            environment
            id
          }
        }
        ... on PerformanceSample {
          id
          appId
          time
          action
          namespace
          duration
          overview {
            key
            value
          }
          incident {
            ... on PerformanceIncident {
              number
            }
          }
          app {
            name
            environment
            id
          }
        }
      }
    }
  }
```
#### Query variables
```graphql
{
  "organizationSlug":"APPLICATION-SLUG" // taken from the URL of the application,
  "sampleType":"EXCEPTION",
  "query": "tag:value" // replace the word value with the values you are searching for
}
```
<script crossorigin src="https://unpkg.com/react/umd/react.production.min.js"></script>
<script crossorigin src="https://unpkg.com/react-dom/umd/react-dom.production.min.js"></script>
<script crossorigin src="https://unpkg.com/graphiql/graphiql.min.js"></script>
<link href="https://unpkg.com/graphiql/graphiql.min.css" rel="stylesheet" />


<script>
  function graphQLFetcher(graphQLParams) {
    let token = document.querySelector('#token').value;
    return fetch(
      `https://appsignal.com/graphql?token=${token}`,
      {
        method: 'post',
        headers: {
          Accept: 'application/json',
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(graphQLParams),
        credentials: 'omit',
      },
    ).then(function (response) {
      return response.json().catch(function () {
        return response.text();
      });
    });
  }

  ReactDOM.render(
    React.createElement(GraphiQL, {
      fetcher: graphQLFetcher,
      defaultVariableEditorOpen: false,
    }),
    document.getElementById('graphiql'),
  );
</script>
