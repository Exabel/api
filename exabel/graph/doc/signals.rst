
Signals
=======

A signal is a type of time series that is connected to exactly one entity type, for instance trade price for listings,
earnings for companies, traffic numbers for web domains.

The collection id for signals is ``signals``.


Get signal
----------
..  http:example:: curl wget python-requests

    GET /v1/signals/visitors HTTP/1.1
    Host: graph.api.exabel.com


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "name": "signals/visitors",
      "entity_type": "entityTypes/stores",
      "display_name": "Daily visitors",
      "description": "The number of visitors in a store per day"
    }


Create signal
-------------
..  http:example:: curl wget python-requests

    POST /v1/signals/visitors HTTP/1.1
    Host: graph.api.exabel.com
    Content-Type: application/json; charset=utf-8

    {
      "entity_type": "entityTypes/stores",
      "display_name": "Daily visitors",
      "description": "The number of visitors in a store per day"
    }


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "name": "signals/visitors",
      "entity_type": "entityTypes/stores",
      "display_name": "Daily visitors",
      "description": "The number of visitors in a store per day"
    }


Update signal
-------------
..  http:example:: curl wget python-requests

    PATCH /v1/signals/visitors HTTP/1.1
    Host: graph.api.exabel.com
    Content-Type: application/json; charset=utf-8

    {
      "entity_type": "entityTypes/stores",
      "display_name": "Daily visitors",
      "description": "The number of visitors in a store per day",
      "update_mask": ["entity_type", "display_name", "description"]
    }


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "name": "signals/visitors",
      "entity_type": "entityTypes/stores",
      "display_name": "Daily visitors",
      "description": "The number of visitors in a store per day"
    }


Delete signal
-------------

..  http:example:: curl wget python-requests

    DELETE /v1/signals/visitors HTTP/1.1
    Host: graph.api.exabel.com


    HTTP/1.1 200 OK
