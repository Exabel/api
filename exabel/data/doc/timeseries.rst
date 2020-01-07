
Time series
===========

Each time series belongs to exactly one pair of a signal and an entity. The entity must be of the same type as defined
in the signal.

By walking across relationship types in the graph when creating time series models, it is possible to aggregate a signal
from a set of child entities to a parent entity that does not normally have that signal.

The resource name for a a time series is on the form
``entityTypes/ns1.entityTypeName/entity/ns2.entityName/signals/ns3.signalName``
An alternative name for the same time series is
``signals/ns3.signalName/entityTypes/ns1.entityTypeName/entity/ns2.entityName``, but the former is the canonical version
which always will be returned by the server.

List time series by entity
--------------------------

..  http:get:: /v1/entityTypes/{entityTypeId}/entity/{entityId}/timeSeries

    :resjsonarr string name: Time series resource name

..  http:example:: curl wget python-requests

    GET /v1/entityTypes/exabel.store/entity/customer1.apple_store_fifth_avenue/timeSeries HTTP/1.1
    Host: data.api.exabel.com


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "name": "entityTypes/exabel.store/entity/customer1.apple_store_fifth_avenue/signals/customer1.visitors",
      "name": "entityTypes/exabel.store/entity/customer1.apple_store_fifth_avenue/signals/customer1.total_spend_amount"
    }


List time series by signal
--------------------------

..  http:get:: /v1/signals/{signalId}/timeSeries

    :resjsonarr string name: Time series resource name

..  http:example:: curl wget python-requests

    GET /v1/signals/customer1.visitors/timeSeries HTTP/1.1
    Host: data.api.exabel.com


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "name": "entityTypes/exabel.store/entity/customer1.apple_store_fifth_avenue/signals/customer1.visitors",
      "name": "entityTypes/exabel.store/entity/customer1.apple_store_grand_central/signals/customer1.visitors",
      "name": "entityTypes/exabel.store/entity/customer1.apple_store_upper_west_side/signals/customer1.visitors"
    }


Get a specific time series
--------------------------

..  http:get:: /v1/entityTypes/{entityTypeId}/entity/{entityId}/signals/{signalId}

    :query timestamp view.timeRange.fromTime: The start point of the time range. By default included in the range.
    :query boolean view.timeRange.excludeFrom: Set to true to exclude the start point from the range.
    :query timestamp view.timeRange.toTime: The end point of the time range. By default excluded from the range.
    :query boolean view.timeRange.includeTo: Set to true to include the end point in the range.

    :resjsonarr string name: Time series resource name
    :resjson array points: Data points

..  http:example:: curl wget python-requests

    GET /v1/entityTypes/exabel.store/entities/customer1.apple_store_fifth_avenue/signals/customer1.visitors?view.timeRange.fromTime=2019-01-01T00:00:00Z&view.timeRange.fromTime=2019-01-03T00:00:00Z&view.timeRange.includeTo=true HTTP/1.1
    Host: data.api.exabel.com


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "name": "entityTypes/exabel.store/entities/customer1.apple_store_fifth_avenue/signals/customer1.visitors",
      "points": [
        {"time": "2019-01-01T00:00:00Z", "value": 1223},
        {"time": "2019-01-02T00:00:00Z", "value": 3435},
        {"time": "2019-01-03T00:00:00Z", "value": 2976}
      ]
    }


Create time series
------------------

..  http:post:: /v1/entityTypes/{entityTypeId}/entity/{entityId}/signals/{signalId}

    :query timestamp view.timeRange.fromTime: The start point of the time range. By default included in the range.
    :query boolean view.timeRange.excludeFrom: Set to true to exclude the start point from the range.
    :query timestamp view.timeRange.toTime: The end point of the time range. By default excluded from the range.
    :query boolean view.timeRange.includeTo: Set to true to include the end point in the range.

    :reqjson array points: Data points

    :resjson string name: Time series resource name
    :resjson array points: Data points

..  http:example:: curl wget python-requests

    POST /v1/entityTypes/exabel.store/entities/customer1.apple_store_fifth_avenue/signals/customer1.visitors?view.timeRange.fromTime=2019-01-01T00:00:00Z&view.timeRange.fromTime=2019-01-03T00:00:00Z&view.timeRange.includeTo=true HTTP/1.1
    Host: data.api.exabel.com
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
      "name": "entityTypes/exabel.store/entities/customer1.apple_store_fifth_avenue/signals/customer1.visitors",
      "points": [
        {"time": "2019-01-01T00:00:00Z", "value": 1223},
        {"time": "2019-01-02T00:00:00Z", "value": 3435},
        {"time": "2019-01-03T00:00:00Z", "value": 2976}
      ]
    }


Update time series
------------------

The data in this request and the existing data are merged together. All points in the request will overwrite
the existing points with the same key, unless the new value is empty, in which case the point will be deleted.

..  http:patch:: /v1/entityTypes/{entityTypeId}/entity/{entityId}/signals/{signalId}

    :query timestamp view.timeRange.fromTime: The start point of the time range. By default included in the range.
    :query boolean view.timeRange.excludeFrom: Set to true to exclude the start point from the range.
    :query timestamp view.timeRange.toTime: The end point of the time range. By default excluded from the range.
    :query boolean view.timeRange.includeTo: Set to true to include the end point in the range.

    :reqjson array points: Data points

    :resjson string name: Time series resource name
    :resjson array points: Data points


..  http:example:: curl wget python-requests

    PATCH /v1/entityTypes/exabel.store/entities/customer1.apple_store_fifth_avenue/signals/customer1.visitors?view.timeRange.fromTime=2019-01-04T00:00:00Z&view.timeRange.fromTime=2019-01-06T00:00:00Z&view.timeRange.includeTo=true HTTP/1.1
    Host: data.api.exabel.com
    Content-Type: application/json; charset=utf-8

    {
      "points": [
        {"time": "2019-01-04T00:00:00Z", "value": 4231},
        {"time": "2019-01-05T00:00:00Z"},
        {"time": "2019-01-06T00:00:00Z", "value": 3521}
      ]
    }


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "name": "entityTypes/exabel.store/entities/customer1.apple_store_fifth_avenue/signals/customer1.visitors",
      "points": [
        {"time": "2019-01-04T00:00:00Z", "value": 4231},
        {"time": "2019-01-06T00:00:00Z", "value": 3521}
      ]
    }


Delete time series points
-------------------------

..  http:post:: /v1/entityTypes/{entityTypeId}/entity/{entityId}/signals/{signalId}/points:batchDelete

    :reqjson array timeRanges: List of time ranges to delete data points from.

..  http:example:: curl wget python-requests

    POST /v1/entityTypes/exabel.store/entities/customer1.apple_store_fifth_avenue/signals/customer1.visitors/points:batchDelete HTTP/1.1
    Host: data.api.exabel.com
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

..  http:delete:: /v1/entityTypes/{entityTypeId}/entity/{entityId}/signals/{signalId}

..  http:example:: curl wget python-requests

    DELETE /v1/entityTypes/exabel.store/entities/customer1.apple_store_fifth_avenue/signals/customer1.visitors HTTP/1.1
    Host: data.api.exabel.com


    HTTP/1.1 200 OK
