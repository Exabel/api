---
title: Time series
category: 61a1198a80c2b80010b68b81
slug: data-api-time-series
---
A _time series_ is a series of data points with corresponding timestamps. Each timestamp may have multiple versions with different values that have evolved over time. Each point therefore has the following structure:
```json
{
  "time": "2021-11-17T00:00:00Z",
  "value": 3.1415,
  "knownTime": "2021-11-19T00:00:00Z",
}
```

Each time series in the Exabel data model is associated with one signal and one entity. For example, a company may have a time series for its revenue. That time series is then associated with that company and with the _revenue_ signal.

By walking across relationship types in the graph when creating time series models, it is possible to aggregate a signal from a set of child entities to a parent entity that does not normally have that signal.

The Exabel Platform currently only supports modelling in daily or lower resolution. Timestamps must be normalised to **midnight UTC**.

The resource name for a a time series is on the form `entityTypes/ns1.entityTypeName/entities/ns2.entityName/signals/ns3.signalName`. An alternative name for the same time series is `signals/ns3.signalName/entityTypes/ns1.entityTypeName/entities/ns2.entityName`, but the former is the canonical version which always will be returned by the server.

Creating and updating time series points are _eventually consistent_.

-[TimeSeriesService REST reference documentation](https://help.exabel.com/reference/timeseriesservice)
-[TimeSeriesService gRPC reference documentation](https://help.exabel.com/docs/data-api-grpc-reference#timeseriesservice)
