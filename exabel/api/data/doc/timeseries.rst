.. _timeseries:

Time series
===========

A `time series` is a series of data points with corresponding timestamps. Each time series in the
Exabel data model is associated with one signal and one entity. For example, a company may have a
time series for its revenue. That time series is then associated with that company and with the
`revenue` signal.

By walking across relationship types in the graph when creating time series models, it is possible
to aggregate a signal from a set of child entities to a parent entity that does not normally have
that signal.

The Exabel Platform currently only supports modelling in daily or higher resolution. Time stamps must
be normalised to **midnight UTC**.

The resource name for a a time series is on the form
``entityTypes/ns1.entityTypeName/entities/ns2.entityName/signals/ns3.signalName``
An alternative name for the same time series is
``signals/ns3.signalName/entityTypes/ns1.entityTypeName/entities/ns2.entityName``, but the former
is the canonical version which always will be returned by the server.

Creating and updating time series points are *eventually consistent*.

List time series by entity
--------------------------

..  http:get:: /v1/entityTypes/{entityTypeId}/entities/{entityId}/timeSeries

    :query int pageSize: The maximum number of results to return. Defaults to 1000, which is also the maximum value
        of this field.
    :query string pageToken: The page token to resume the results from, as returned from a previous request to this
        method with the same query parameters.
    :resjson object timeSeries: The resulting time series.
    :resjson string nextPageToken: The page token where the list continues. Can be sent to a subsequent query.
    :resjson int totalSize: The total number of results, irrespective of paging.

..  http:example:: curl wget python-requests

    GET /v1/entityTypes/store/entities/customer1.apple_store_fifth_avenue/timeSeries HTTP/1.1
    Host: data.api.exabel.com
    Accept: application/json
    X-Api-Key: API_KEY_GOES_HERE


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "timeSeries":
        [
          { "name": "entityTypes/store/entities/customer1.apple_store_fifth_avenue/signals/customer1.visitors" },
          { "name": "entityTypes/store/entities/customer1.apple_store_fifth_avenue/signals/customer1.total_spend_amount" }
        ],
      "nextPageToken": "graph:entityTypes:store:entities:customer1:apple_store_fifth_avenue:signals:customer1:total_spend_amount",
      "totalSize": 2
    }


List time series by signal
--------------------------

..  http:get:: /v1/signals/{signalId}/timeSeries

    :query int pageSize: The maximum number of results to return. Defaults to 1000, which is also the maximum value
        of this field.
    :query string pageToken: The page token to resume the results from, as returned from a previous request to this
        method with the same query parameters.
    :resjson object timeSeries: The resulting time series.
    :resjson string nextPageToken: The page token where the list continues. Can be sent to a subsequent query.
    :resjson int totalSize: The total number of results, irrespective of paging.

..  http:example:: curl wget python-requests

    GET /v1/signals/customer1.visitors/timeSeries HTTP/1.1
    Host: data.api.exabel.com
    Accept: application/json
    X-Api-Key: API_KEY_GOES_HERE


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "timeSeries":
        [
          { "name": "entityTypes/store/entities/customer1.apple_store_fifth_avenue/signals/customer1.visitors" },
          { "name": "entityTypes/store/entities/customer1.apple_store_grand_central/signals/customer1.visitors" },
          { "name": "entityTypes/store/entities/customer1.apple_store_upper_west_side/signals/customer1.visitors" }
        ],
      "nextPageToken": "graph:entityTypes:store:entities:customer1:apple_store_upper_west_side:signals:customer1:visitors",
      "totalSize": 3
    }


Get a specific time series
--------------------------

..  http:get:: /v1/entityTypes/{entityTypeId}/entities/{entityId}/signals/{signalId}

    :query timestamp view.timeRange.fromTime: The start point of the time range. By default included in the range.
    :query boolean view.timeRange.excludeFrom: Set to true to exclude the start point from the range.
    :query timestamp view.timeRange.toTime: The end point of the time range. By default excluded from the range.
    :query boolean view.timeRange.includeTo: Set to true to include the end point in the range.

    :resjsonarr string name: Time series resource name
    :resjson array points: Data points

..  http:example:: curl wget python-requests

    GET /v1/entityTypes/store/entities/customer1.apple_store_fifth_avenue/signals/customer1.visitors?view.timeRange.fromTime=2019-01-01T00:00:00Z&view.timeRange.toTime=2019-01-03T00:00:00Z&view.timeRange.includeTo=true HTTP/1.1
    Host: data.api.exabel.com
    Accept: application/json
    X-Api-Key: API_KEY_GOES_HERE


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "name": "entityTypes/store/entities/customer1.apple_store_fifth_avenue/signals/customer1.visitors",
      "points": [
        {"time": "2019-01-01T00:00:00Z", "value": 1223},
        {"time": "2019-01-02T00:00:00Z", "value": 3435},
        {"time": "2019-01-03T00:00:00Z", "value": 2976}
      ]
    }


Create time series
------------------

**Note** Time series points are stored with second resolution, however the Exabel Platform only supports processing
time series with daily or higher resolution. Time stamps must be normalised to **midnight UTC** (``00:00:00Z``).

..  http:post:: /v1/entityTypes/{entityTypeId}/entities/{entityId}/signals/{signalId}

    :query timestamp view.timeRange.fromTime: The start point of the time range. By default included in the range.
    :query boolean view.timeRange.excludeFrom: Set to true to exclude the start point from the range.
    :query timestamp view.timeRange.toTime: The end point of the time range. By default excluded from the range.
    :query boolean view.timeRange.includeTo: Set to true to include the end point in the range.
    :query boolean createTag: Set to true to create a tag for every entity type a signal has time series for.
      If a tag already exists, it will be updated when time series are created (or deleted) regardless of the value
      of this flag.

    :reqjson array points: Data points

    :resjson string name: Time series resource name
    :resjson array points: Data points

..  http:example:: curl wget python-requests

    POST /v1/entityTypes/store/entities/customer1.apple_store_fifth_avenue/signals/customer1.visitors?view.timeRange.fromTime=2019-01-01T00:00:00Z&view.timeRange.toTime=2019-01-03T00:00:00Z&view.timeRange.includeTo=true HTTP/1.1
    Host: data.api.exabel.com
    Accept: application/json
    X-Api-Key: API_KEY_GOES_HERE
    Content-Type: application/json; charset=utf-8

    {
      "points": [
        {"time": "2019-01-01T00:00:00Z", "value": 1223},
        {"time": "2019-01-02T00:00:00Z", "value": 3435},
        {"time": "2019-01-03T00:00:00Z", "value": 2976}
      ]
    }


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "name": "entityTypes/store/entities/customer1.apple_store_fifth_avenue/signals/customer1.visitors",
      "points": [
        {"time": "2019-01-01T00:00:00Z", "value": 1223},
        {"time": "2019-01-02T00:00:00Z", "value": 3435},
        {"time": "2019-01-03T00:00:00Z", "value": 2976}
      ]
    }


Update time series
------------------

The data in this request and the existing data are merged together. All points in the request will overwrite
the existing points with the same key. If a point that is previously updated is not included, it is **not** deleted,
even though if it is within the range of this update. Data points without values are ignored.

**Note** Time series points are stored with second resolution, however the Exabel Platform only supports processing
time series with daily or higher resolution. Time stamps must be normalised to **midnight UTC** (```00:00:00Z``).

..  http:patch:: /v1/entityTypes/{entityTypeId}/entities/{entityId}/signals/{signalId}

    :query timestamp view.timeRange.fromTime: The start point of the time range. By default included in the range.
    :query boolean view.timeRange.excludeFrom: Set to true to exclude the start point from the range.
    :query timestamp view.timeRange.toTime: The end point of the time range. By default excluded from the range.
    :query boolean view.timeRange.includeTo: Set to true to include the end point in the range.

    :reqjson array points: Data points

    :resjson string name: Time series resource name
    :resjson array points: Data points


..  http:example:: curl wget python-requests

    PATCH /v1/entityTypes/store/entities/customer1.apple_store_fifth_avenue/signals/customer1.visitors?view.timeRange.fromTime=2019-01-04T00:00:00Z&view.timeRange.toTime=2019-01-06T00:00:00Z&view.timeRange.includeTo=true HTTP/1.1
    Host: data.api.exabel.com
    Accept: application/json
    X-Api-Key: API_KEY_GOES_HERE
    Content-Type: application/json; charset=utf-8

    {
      "points": [
        {"time": "2019-01-04T00:00:00Z", "value": 4231},
        {"time": "2019-01-06T00:00:00Z", "value": 3521}
      ]
    }


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "name": "entityTypes/store/entities/customer1.apple_store_fifth_avenue/signals/customer1.visitors",
      "points": [
        {"time": "2019-01-04T00:00:00Z", "value": 4231},
        {"time": "2019-01-06T00:00:00Z", "value": 3521}
      ]
    }


Delete time series points
-------------------------

..  http:post:: /v1/entityTypes/{entityTypeId}/entities/{entityId}/signals/{signalId}/points:batchDelete

    :reqjson array timeRanges: List of time ranges to delete data points from.

..  http:example:: curl wget python-requests

    POST /v1/entityTypes/store/entities/customer1.apple_store_fifth_avenue/signals/customer1.visitors/points:batchDelete HTTP/1.1
    Host: data.api.exabel.com
    Accept: application/json
    X-Api-Key: API_KEY_GOES_HERE
    Content-Type: application/json; charset=utf-8

    {
      "timeRanges": [
        {
          "fromTime": "2019-01-04T00:00:00Z",
          "excludeFrom": "true",
          "toTime": "2019-01-05T00:00:00Z",
          "includeTo": "true"
        }
      ]
    }


    HTTP/1.1 200 OK


Delete time series
------------------

..  note:: This will delete **all** points in the time series.

..  http:delete:: /v1/entityTypes/{entityTypeId}/entities/{entityId}/signals/{signalId}

..  http:example:: curl wget python-requests

    DELETE /v1/entityTypes/store/entities/customer1.apple_store_fifth_avenue/signals/customer1.visitors HTTP/1.1
    Host: data.api.exabel.com
    Accept: application/json
    X-Api-Key: API_KEY_GOES_HERE


    HTTP/1.1 200 OK
