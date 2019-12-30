
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
..  http:example:: curl wget python-requests

    GET /v1/entityTypes/store/entity/apple_store_fifth_avenue/timeSeries HTTP/1.1
    Host: graph.api.exabel.com


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "name": "entityTypes/store/entity/apple_store_fifth_avenue/signals/visitors",
      "name": "entityTypes/store/entity/apple_store_fifth_avenue/signals/total_spend_amount"
    }


List time series by signal
--------------------------
..  http:example:: curl wget python-requests

    GET /v1/signals/visitors/timeSeries HTTP/1.1
    Host: graph.api.exabel.com


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "name": "entityTypes/store/entity/apple_store_fifth_avenue/signals/visitors",
      "name": "entityTypes/store/entity/apple_store_grand_central/signals/visitors",
      "name": "entityTypes/store/entity/apple_store_upper_west_side/signals/visitors"
    }


Get time series
---------------
..  http:example:: curl wget python-requests

    GET /v1/entityTypes/store/entities/apple_store_fifth_avenue/signals/visitors HTTP/1.1
    Host: graph.api.exabel.com


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "name": "entityTypes/store/entities/apple_store_fifth_avenue/signals/visitors",
      "points": [
        {"time": "2019-01-01T00:00:00Z", "value": 1223},
        {"time": "2019-01-02T00:00:00Z", "value": 3435},
        {"time": "2019-01-03T00:00:00Z", "value": 2976}
      ]
    }


Create time series
------------------
..  http:example:: curl wget python-requests

    POST /v1/entityTypes/store/entities/apple_store_fifth_avenue/signals/visitors HTTP/1.1
    Host: graph.api.exabel.com
    Content-Type: application/json; charset=utf-8

    {
      "points": [
        {"time": "2019-01-01T00:00:00Z", "value": 1223},
        {"time": "2019-01-02T00:00:00Z", "value": 3435},
        {"time": "2019-01-03T00:00:00Z", "value": 2976}
      ],
      "view": {
        "time_range": {
          "from_time": "2019-01-01T00:00:00Z",
          "to_time": "2019-01-03T00:00:00Z",
          "include_to": "true"
        }
      }
    }


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "name": "entityTypes/store/entities/apple_store_fifth_avenue/signals/visitors",
      "points": [
        {"time": "2019-01-01T00:00:00Z", "value": 1223},
        {"time": "2019-01-02T00:00:00Z", "value": 3435},
        {"time": "2019-01-03T00:00:00Z", "value": 2976}
      ]
    }


Update time series
------------------
..  http:example:: curl wget python-requests

    PATCH /v1/entityTypes/store/entities/apple_store_fifth_avenue/signals/visitors HTTP/1.1
    Host: graph.api.exabel.com
    Content-Type: application/json; charset=utf-8

    {
      "points": [
        {"time": "2019-01-04T00:00:00Z", "value": 4231},
        {"time": "2019-01-05T00:00:00Z", "value": 3121},
        {"time": "2019-01-06T00:00:00Z", "value": 3521}
      ],
     "view": {
      "time_range": {
        "from_time": "2019-01-04T00:00:00Z",
        "to_time": "2019-01-06T00:00:00Z",
        "include_to": "true"
      }
     }
    }


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "name": "entityTypes/store/entities/apple_store_fifth_avenue/signals/visitors",
      "points": [
        {"time": "2019-01-04T00:00:00Z", "value": 4231},
        {"time": "2019-01-05T00:00:00Z", "value": 3121},
        {"time": "2019-01-06T00:00:00Z", "value": 3521}
      ]
    }


Delete time series points
-------------------------

..  http:example:: curl wget python-requests

    DELETE /v1/entityTypes/store/entities/apple_store_fifth_avenue/signals/visitors/points:batchDelete HTTP/1.1
    Host: graph.api.exabel.com
    Content-Type: application/json; charset=utf-8

    {
      "time_ranges": [
        {
          "from_time": "2019-01-04T00:00:00Z",
          "exclude_from": "true",
          "to_time": "2019-01-05T00:00:00Z",
          "include_to": "true"
        }
      ]
    }


    HTTP/1.1 200 OK


Delete time series
------------------

..  note:: This will delete **all** points in the time series.

..  http:example:: curl wget python-requests

    DELETE /v1/entityTypes/store/entities/apple_store_fifth_avenue/signals/visitors HTTP/1.1
    Host: graph.api.exabel.com


    HTTP/1.1 200 OK
