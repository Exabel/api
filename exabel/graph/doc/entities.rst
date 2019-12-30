
Entities
========

Entity types
************

An *entity type* is a group of entities having similar real world meaning, for instance company, web domain, brand,
or region.

The collection id for entity types is ``entityTypes``.


List entity types
-----------------

Retrieves the entity type catalogue.

..  http:example:: curl wget python-requests

    GET /v1/entityTypes HTTP/1.1
    Host: graph.api.exabel.com


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    [
      {
        "name": "entityTypes/brand"
      },
      {
        "name": "entityTypes/region"
      }
    ]


Get entity type details
-----------------------

..  http:example:: curl wget python-requests

    GET /v1/entityTypes/brand HTTP/1.1
    Host: graph.api.exabel.com


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "name": "entityTypes/brand",
      "display_name": "Brand",
      "description": "Brands owned by companies"
    }


Entities
********

An *entity* belongs to exactly one entity type and is usually a real-world instance of its type. They are created
and managed either by a customer or by Exabel, for instance Alphabet, Inc., www.amazon.com, Coca-Cola, EMEA.
Entities are the core concept of this API.

The collection id for entities is ``entities``


List entities
-------------

Retrieves a list of all entities of a given entity type.

..  http:example:: curl wget python-requests

    GET /v1/entityTypes/brand/entities HTTP/1.1
    Host: graph.api.exabel.com


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    [
      {
        "name": "entityTypes/brand/entities/audi"
      },
      {
        "name": "entityTypes/brand/entities/vw"
      },
      {
        "name": "entityTypes/brand/entities/seat"
      },
      {
        "name": "entityTypes/brand/entities/skoda"
      }
    ]


Get entity
----------
..  http:example:: curl wget python-requests

    GET /v1/entityTypes/brand/entities/skoda HTTP/1.1
    Host: graph.api.exabel.com


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

      {
        "name": "entityTypes/brand/entities/skoda",
        "display_name": "Škoda"
      }


Create entity
-------------
..  http:example:: curl wget python-requests

    POST /v1/entityTypes/brand/entities/skoda HTTP/1.1
    Host: graph.api.exabel.com
    Content-Type: application/json; charset=utf-8

    {
      "name": "entityTypes/brand/entities/skoda",
      "display_name": "Škoda"
    }


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "name": "entityTypes/brand/entities/skoda",
      "display_name": "Škoda"
    }


Update entity
-------------
..  http:example:: curl wget python-requests

    PUT /v1/entityTypes/brand/entities/skoda HTTP/1.1
    Host: graph.api.exabel.com
    Content-Type: application/json; charset=utf-8

    {
      "name": "entityTypes/brand/entities/skoda",
      "display_name": "Škoda",
      "description": "Simply clever"
    }


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "name": "entityTypes/brand/entities/skoda",
      "display_name": "Škoda",
      "description": "Simply clever"
    }


Delete entity
-------------

..  note:: **All** relationships and time series for this entity will also be deleted.

..  http:example:: curl wget python-requests

    DELETE /v1/entityTypes/brand/entities/skoda HTTP/1.1
    Host: graph.api.exabel.com


    HTTP/1.1 200 OK


.. note:: TODO: Add examples with fieldmask for updating properties?