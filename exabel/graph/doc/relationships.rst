
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

..  http:get:: /v1/relationshipTypes

    :>jsonarr string name: Relationship type resource name

..  http:example:: curl wget python-requests

    GET /v1/relationshipTypes HTTP/1.1
    Host: graph.api.exabel.com


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    [
      {
        "name": "relationshipTypes/exabel.HAS_BRAND"
      },
      {
        "name": "relationshipTypes/customer1.PRODUCED_AT"
      }
    ]


Get relationship type details
-----------------------------

..  http:get:: /v1/relationshipTypes/{relationshipTypeId}

    :>json string name: Relationship type resource name
    :>json string description: Relationship type description
    :>json object properties: Relationship type properties

..  http:example:: curl wget python-requests

    GET /v1/relationshipTypes/exabel.HAS_BRAND HTTP/1.1
    Host: graph.api.exabel.com


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "name": "relationshipTypes/exabel.HAS_BRAND",
      "description": "Denotes a company that owns a brand"
    }



Create relationship type
------------------------

..  http:post:: /v1/relationshipTypes

    :<json string name: Relationship type resource name on the format ``relationshipTypes/{relationshipTypeId}``
        (required)
    :<json string description: Relationship type description
    :<json object properties: Relationship type properties

    :>json string name: Relationship type resource name
    :>json string description: Relationship type description
    :>json object properties: Relationship type properties

..  http:example:: curl wget python-requests

    POST /v1/relationshipTypes HTTP/1.1
    Host: graph.api.exabel.com
    Content-Type: application/json; charset=utf-8

    {
      "name": "relationshipTypes/exabel.HAS_BRAND",
      "description": "Denotes a company that owns a brand"
    }


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "name": "relationshipTypes/exabel.HAS_BRAND",
      "description": "Denotes a company that owns a brand"
    }


Update relationship type
------------------------

..  http:patch:: /v1/relationshipTypes/{relationshipTypeId}

    :<json string description: Relationship type description
    :<json object properties: Relationship type properties
    :<json array updateMask: Field mask (required)

    :>json string name: Relationship type resource name
    :>json string description: Relationship type description
    :>json object properties: Relationship type properties

..  http:example:: curl wget python-requests

    PATCH /v1/relationshipTypes/exabel.HAS_BRAND HTTP/1.1
    Host: graph.api.exabel.com
    Content-Type: application/json; charset=utf-8

    {
      "description": "Denotes a company that owns a brand",
      "updateMask": ["description"]
    }


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "name": "relationshipTypes/exabel.HAS_BRAND",
      "description": "Denotes a company that owns a brand"
    }


Delete relationship type
------------------------

Delete is not supported by the API. If you need to delete a relationship type, contact support@exabel.com.


Relationships
*************

A *relationship* belongs to exactly one relationship type and defines a directed relationship between two concrete
entities. For two specific entities, there can be at most one relationship of the same type between them.

Relationships created and managed by Exabel are exclusively between Exabel’s entities.

Relationships created and managed by a customer are between their and Exabel’s entities in any combination.

The collection id for relationships is ``relationships``.


List relationships
------------------

..  http:get:: /v1/relationshipTypes/{relationshipTypeId}/relationships

    :query fromEntity: The entity resource name of the start point of the relationship on the form
        ``entityTypes/{entityTypeId}}/entities/{entityId}``
    :query toEntity: The entity resource name of the end point of the relationship on the form
        ``entityTypes/{entityTypeId}}/entities/{entityId}``

    At least one of ``fromEntity`` and ``toEntity`` must be provided.

    Use ``-`` for ``relationshipTypeId`` to get relationships for all types.

    :>jsonarr string parent: Relationship type resource name
    :>jsonarr string fromEntity: The entity resource name of the start point of the relationship
    :>jsonarr string toEntity: The entity resource name of the end point of the relationship

    To get *all* relationships between two entities, perform the request a second time with ``fromEntity`` and
    ``toEntity`` swapped.

..  http:example:: curl wget python-requests

    GET /v1/relationshipTypes/exabel.HAS_BRAND/relationships?fromEntity=entityTypes/exabel.company/entities/exabel.001yfz_e-volkswagen_ag HTTP/1.1
    Host: graph.api.exabel.com


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    [
        {
          "parent": "relationshipTypes/exabel.HAS_BRAND",
          "fromEntity": "entityTypes/exabel.company/entities/exabel.001yfz_e-volkswagen_ag",
          "toEntity": "entityTypes/exabel.brand/entities/customer1.skoda"
        },
        {
          "parent": "relationshipTypes/exabel.HAS_BRAND",
          "fromEntity": "entityTypes/exabel.company/entities/exabel.001yfz_e-volkswagen_ag",
          "toEntity": "entityTypes/exabel.brand/entities/customer1.audi"
        },
        {
          "parent": "relationshipTypes/exabel.HAS_BRAND",
          "fromEntity": "entityTypes/exabel.company/entities/exabel.001yfz_e-volkswagen_ag",
          "toEntity": "entityTypes/exabel.brand/entities/customer1.vw"
        }
    ]


Get relationship
----------------

..  http:get:: /v1/relationshipTypes/{relationshipTypeId}/relationships

    :query fromEntity: The entity resource name of the start point of the relationship on the form
        ``entityTypes/{entityTypeId}}/entities/{entityId}`` (required)
    :query toEntity: The entity resource name of the end point of the relationship on the form
        ``entityTypes/{entityTypeId}}/entities/{entityId}`` (required)

    :>json string parent: Relationship type resource name
    :>json string fromEntity: The entity resource name of the start point of the relationship
    :>json string toEntity: The entity resource name of the end point of the relationship
    :>json string description: Relationship description
    :>json object properties: Relationship properties

..  http:example:: curl wget python-requests

    GET /v1/relationshipTypes/exabel.HAS_BRAND/relationships?fromEntity=entityTypes/exabel.company/entities/exabel.001yfz_e-volkswagen_ag&toEntity=entityTypes/exabel.brand/entities/customer1.skoda HTTP/1.1
    Host: graph.api.exabel.com


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "parent": "relationshipTypes/exabel.HAS_BRAND",
      "fromEntity": "entityTypes/exabel.company/entities/exabel.001yfz_e-volkswagen_ag",
      "toEntity": "entityTypes/exabel.brand/entities/customer1.skoda",
      "description": "Škoda is a brand of Volkswagen AG"
    }



Create relationship
-------------------
..  http:post:: /v1/relationshipTypes/{relationshipTypeId}/relationships

    :<json string fromEntity: The entity resource name of the start point of the relationship (required)
    :<json string toEntity: The entity resource name of the end point of the relationship (required)
    :<json string description: Relationship description
    :<json object properties: Relationship properties

    :>json string parent: Relationship type resource name
    :>json string fromEntity: The entity resource name of the start point of the relationship
    :>json string toEntity: The entity resource name of the end point of the relationship
    :>json string description: Relationship description
    :>json object properties: Relationship properties

..  http:example:: curl wget python-requests

    POST /v1/relationshipTypes/exabel.HAS_BRAND/relationships HTTP/1.1
    Host: graph.api.exabel.com
    Content-Type: application/json; charset=utf-8

    {
      "fromEntity": "entityTypes/exabel.company/entities/exabel.001yfz_e-volkswagen_ag",
      "toEntity": "entityTypes/exabel.brand/entities/customer1.skoda",
      "description": "Škoda is a brand of Volkswagen AG"
    }


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "parent": "relationshipTypes/exabel.HAS_BRAND",
      "fromEntity": "entityTypes/exabel.company/entities/exabel.001yfz_e-volkswagen_ag",
      "toEntity": "entityTypes/exabel.brand/entities/customer1.skoda",
      "description": "Škoda is a brand of Volkswagen AG"
    }


Update relationship
-------------------
..  http:put:: /v1/relationshipTypes/{relationshipTypeId}/relationships

    :<json string fromEntity: The entity resource name of the start point of the relationship (required)
    :<json string toEntity: The entity resource name of the end point of the relationship (required)
    :<json string description: Relationship description
    :<json object properties: Relationship properties

    :>json string parent: Relationship type resource name
    :>json string fromEntity: The entity resource name of the start point of the relationship
    :>json string toEntity: The entity resource name of the end point of the relationship
    :>json string description: Relationship description
    :>json object properties: Relationship properties

..  http:example:: curl wget python-requests

    PUT /v1/relationshipTypes/exabel.HAS_BRAND/relationships HTTP/1.1
    Host: graph.api.exabel.com
    Content-Type: application/json; charset=utf-8

    {
      "fromEntity": "entityTypes/exabel.company/entities/exabel.001yfz_e-volkswagen_ag",
      "toEntity": "entityTypes/exabel.brand/entities/customer1.skoda",
      "description": "Škoda is a brand of Volkswagen AG",
      "properties": {
        "ownedSince": "1994-12-19"
      }
    }


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "parent": "relationshipTypes/exabel.HAS_BRAND",
      "fromEntity": "entityTypes/exabel.company/entities/exabel.001yfz_e-volkswagen_ag",
      "toEntity": "entityTypes/exabel.brand/entities/customer1.skoda",
      "description": "Škoda is a brand of Volkswagen AG",
      "properties": {
        "ownedSince": "1994-12-19"
      }
    }


Delete relationship
-------------------
..  http:delete:: /v1/relationshipTypes/{relationshipTypeId}/relationships

    :query fromEntity: entityTypes/{entityTypeId}}/entities/{entityId} (required)
    :query toEntity: entityTypes/{entityTypeId}}/entities/{entityId} (required)

..  http:example:: curl wget python-requests

    DELETE /v1/relationshipTypes/exabel.HAS_BRAND/relationships?fromEntity=entityTypes/exabel.company/entities/exabel.001yfz_e-volkswagen_ag&toEntity=entityTypes/exabel.brand/entities/customer1.skoda HTTP/1.1
    Host: graph.api.exabel.com


    HTTP/1.1 200 OK
