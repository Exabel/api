
Relationships
=============


Relationship types
******************

A *relationship type* is a type of relationship between entities, for instance ``HAS_BRAND``, ``LOCATED_IN``,
or ``OWNED_BY``. They are created and managed either by a customer or by Exabel. Usually, a relationship type makes
sense only between specific entity types (a ``store`` is ``LOCATED_IN`` a ``city``), but they can apply to any pair
of entities where they would fit.

The collection id for relationship types is ``relationshipTypes``.

List relationship types
-----------------------

Retrieves the relationship type catalogue.

..  http:example:: curl wget python-requests

    GET /v1/relationshipTypes HTTP/1.1
    Host: graph.api.exabel.com


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    [
      {
        "name": "relationshipTypes/HAS_BRAND"
      },
      {
        "name": "relationshipTypes/LOCATED_IN"
      }
    ]


Get relationship type details
-----------------------------

..  http:example:: curl wget python-requests

    GET /v1/relationshipTypes/HAS_BRAND HTTP/1.1
    Host: graph.api.exabel.com


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "name": "relationshipTypes/HAS_BRAND",
      "description": "Denotes a company that owns a brand"
    }



Create relationship type
------------------------
..  http:example:: curl wget python-requests

    POST /v1/relationshipTypes/HAS_BRAND HTTP/1.1
    Host: graph.api.exabel.com
    Content-Type: application/json; charset=utf-8

    {
      "name": "relationshipTypes/HAS_BRAND",
      "description": "Denotes a company that owns a brand"
    }


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "name": "relationshipTypes/HAS_BRAND",
      "description": "Denotes a company that owns a brand"
    }


Update relationship type
------------------------
..  http:example:: curl wget python-requests

    PUT /v1/relationshipTypes/HAS_BRAND HTTP/1.1
    Host: graph.api.exabel.com
    Content-Type: application/json; charset=utf-8

    {
      "name": "relationshipTypes/HAS_BRAND",
      "description": "Denotes a company that owns a brand"
    }


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "name": "relationshipTypes/HAS_BRAND",
      "description": "Denotes a company that owns a brand"
    }


Delete relationship type
------------------------

Delete is not supported by the API. If you need to delete a relationship type, contact support@exabel.com.


Relationships
*************

A *relationship* belongs to exactly one relationship type and defines a directed relationship between two concrete
entities. For two specific entities, there should be at most one relationship of the same type between them.

Relationships created and managed by Exabel are exclusively between Exabel’s entities.

Relationships created and managed by a customer are between their and Exabel’s entities in any combination.

The collection id for relationships is ``relationships``.


Get relationship
----------------

..  http:example:: curl wget python-requests

    GET /v1/relationshipTypes/HAS_BRAND/relationships?from_entity=entityTypes/company/entities/001yfz_e-volkswagen_ag&to_entity=entityTypes/brand/entities/skoda HTTP/1.1
    Host: graph.api.exabel.com


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "parent": "relationshipTypes/HAS_BRAND",
      "from_entity": "entityTypes/company/entities/001yfz_e-volkswagen_ag",
      "to_entity": "entityTypes/brand/entities/skoda",
      "description": "Škoda is a brand of Volkswagen AG"
    }



Create relationship
-------------------
..  http:example:: curl wget python-requests

    POST /v1/relationshipTypes/HAS_BRAND/relationships HTTP/1.1
    Host: graph.api.exabel.com
    Content-Type: application/json; charset=utf-8

    {
      "parent": "relationshipTypes/HAS_BRAND",
      "from_entity": "entityTypes/company/entities/001yfz_e-volkswagen_ag",
      "to_entity": "entityTypes/brand/entities/skoda",
      "description": "Škoda is a brand of Volkswagen AG"
    }


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "parent": "relationshipTypes/HAS_BRAND",
      "from_entity": "entityTypes/company/entities/001yfz_e-volkswagen_ag",
      "to_entity": "entityTypes/brand/entities/skoda",
      "description": "Škoda is a brand of Volkswagen AG"
    }


Update relationship
-------------------
..  http:example:: curl wget python-requests

    PUT /v1/relationshipTypes/HAS_BRAND/relationships HTTP/1.1
    Host: graph.api.exabel.com
    Content-Type: application/json; charset=utf-8

    {
      "parent": "relationshipTypes/HAS_BRAND",
      "from_entity": "entityTypes/company/entities/001yfz_e-volkswagen_ag",
      "to_entity": "entityTypes/brand/entities/skoda",
      "description": "Škoda is a brand of Volkswagen AG"
    }


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "parent": "relationshipTypes/HAS_BRAND",
      "from_entity": "entityTypes/company/entities/001yfz_e-volkswagen_ag",
      "to_entity": "entityTypes/brand/entities/skoda",
      "description": "Škoda is a brand of Volkswagen AG"
    }


Delete relationship
-------------------
..  http:example:: curl wget python-requests

    DELETE /v1/relationshipTypes/HAS_BRAND/relationships?from_entity=entityTypes/company/entities/001yfz_e-volkswagen_ag&to_entity=entityTypes/brand/entities/skoda HTTP/1.1
    Host: graph.api.exabel.com


    HTTP/1.1 200 OK
