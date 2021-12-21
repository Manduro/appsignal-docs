---
title: "GraphQL Examples"
---
Here are some of the query examples to get you started. You the use the GraphQL explorer [here](graphql-explorer.html) to run these queries.

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

### Fetch error count

#### Query
```graphql
query MetricsListQuery($appId: String!, $start: DateTime, $end: DateTime, $timeframe: TimeframeEnum, $query: [MetricAggregation!]!) {
    app(id: $appId) {
      id
      metrics {
        list(start: $start, end: $end, query: $query, timeframe: $timeframe) {
          start
          end
          rows {
            name
            tags {
              key
              value
            }
            fields {
              key
              value
            }
          }
        }
      }
    }
  }
```

#### Query variables
```graphql
{
  "appId": "YOUR-APP-ID",
  "start": "2021-06-04T13:00:00.000Z", // change this to the date of your preference
  "end": "2021-12-21T14:00:00.000Z",   // change this to the date of your preference
  "query": [
    {
    "name": "transaction_exception_count",
    "tags": [{ "key": "namespace", "value": "*" }],
    "fields": [{ "field": "COUNTER", "aggregate": "SUM" }]
    }
  ]
}
```