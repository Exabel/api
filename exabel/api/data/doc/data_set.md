---
title: Data sets
category: 61a1198a80c2b80010b68b81
slug: data-api-data-sets
---
A _data set_ is collection of signals, such as _price_ and _revenue_, those signals' time series and a graph of entities, starting from entities having those time series.

A data set is uniquely defined by the collection of its signals. The graph of entities in the data set is built following relationships of _ownership_/_aggregation_ type, for example `HAS_BRAND`, coming from other entities *into* entities with corresponding time series, and expanded further with ownership relations coming into the resulting graph, et cetera until no more ownership relations can be found. (An _ownership_ relationship type has its `isOwnership` field set to `true`.)

The graph of a data set is not necessarily connected.

There are no data sets in the global namespace. Customers can only create data sets in their own namespace.

All signals of a data set must exist when a data set is created. The collection of signals in a data set can be modified later. The signals are to be treated as a _set_, i.e. signals are not repeated and can be inserted or returned in any order.

The collection id for signals is `dataSets`.

-[DataSetService REST reference documentation](https://help.exabel.com/reference/datasetservice)
-[DataSetService gRPC reference documentation](https://help.exabel.com/docs/data-api-grpc-reference#datasetservice)
