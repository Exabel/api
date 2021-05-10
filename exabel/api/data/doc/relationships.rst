
Relationships
=============


Relationship types
******************

Every relationship has a `relationship type`, such as `HAS_BRAND`, `LOCATED_IN` or `OWNED_BY`. The
full resource name for a relationship type is `relationshipTypes/ns.NAME`.

The relationship types provided by Exabel are the following:

.. list-table:: Relationship types
    :widths: 25 40 20
    :header-rows: 1

    * - Relationship type
      - Resource name
      - Typical connected entities
    * - listing
      - ``relationshipTypes/HAS_LISTING``
      - regional → listing
    * - primary listing
      - ``relationshipTypes/HAS_PRIMARY_LISTING``
      - regional → listing
    * - primary regional
      - ``relationshipTypes/HAS_PRIMARY_REGIONAL``
      - security → regional
    * - regional
      - ``relationshipTypes/HAS_REGIONAL``
      - security → regional
    * - security
      - ``relationshipTypes/HAS_SECURITY``
      - company → security
    * - location
      - ``relationshipTypes/LOCATED_IN``
      - company → country
    * - web domain
      - ``relationshipTypes/WEB_DOMAIN_OWNED_BY``
      - company → web domain


The relationship types provided by Exabel are read-only, meaning that customers cannot add new
relationships with those relationship types. Customers are free to create new relationship types in
their own namespace, using the API.

The collection id for relationship types is ``relationshipTypes``.

List relationship types
-----------------------

Retrieves the relationship type catalogue.

..  http:get:: /v1/relationshipTypes

    :query int pageSize: The maximum number of results to return. Defaults to 1000, which is also the maximum value
        of this field.
    :query string pageToken: The page token to resume the results from, as returned from a previous request to this
        method with the same query parameters.
    :resjson array relationshipTypes: The resulting relationship types.
    :resjson string nextPageToken: The page token where the list continues. Can be sent to a subsequent query.
    :resjson int totalSize: The total number of results, irrespective of paging.

..  http:example:: curl wget python-requests

    GET /v1/relationshipTypes HTTP/1.1
    Host: data.api.exabel.com
    Accept: application/json
    X-Api-Key: API_KEY_GOES_HERE


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "relationshipTypes":
        [
          {
            "name": "relationshipTypes/HAS_BRAND",
            "description": "Denotes a company that owns a brand.",
            "readOnly": true,
            "properties": {}
          },
          {
            "name": "relationshipTypes/customer1.PRODUCED_AT",
            "description": "Denotes a factory that manufactures a brand.",
            "properties": {}
          }
        ],
       "nextPageToken": "graph:relationshipType:customer1:PRODUCED_AT",
       "totalSize": 2
    }


Get relationship type details
-----------------------------

..  http:get:: /v1/relationshipTypes/{relationshipTypeId}

    :resjson string name: Relationship type resource name.
    :resjson string description: Relationship type description.
    :resjson boolean readOnly: Whether this resource is read only.
    :resjson object properties: Relationship type properties.

..  http:example:: curl wget python-requests

    GET /v1/relationshipTypes/HAS_BRAND HTTP/1.1
    Host: data.api.exabel.com
    Accept: application/json
    X-Api-Key: API_KEY_GOES_HERE


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "name": "relationshipTypes/HAS_BRAND",
      "description": "Denotes a company that owns a brand",
      "readOnly": true,
      "properties": {}
    }



Create relationship type
------------------------

..  http:post:: /v1/relationshipTypes

    :reqjson string name: Relationship type resource name on the format ``relationshipTypes/{relationshipTypeId}``
        (required).
    :reqjson string description: Relationship type description.
    :reqjson object properties: Relationship type properties.

    :resjson string name: Relationship type resource name.
    :resjson string description: Relationship type description.
    :resjson object properties: Relationship type properties.

..  http:example:: curl wget python-requests

    POST /v1/relationshipTypes HTTP/1.1
    Host: data.api.exabel.com
    Accept: application/json
    X-Api-Key: API_KEY_GOES_HERE
    Content-Type: application/json; charset=utf-8

    {
      "name": "relationshipTypes/HAS_BRAND",
      "description": "Denotes a company that owns a brand"
    }


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "name": "relationshipTypes/HAS_BRAND",
      "description": "Denotes a company that owns a brand",
      "properties": {}
    }


Update relationship type
------------------------

..  http:patch:: /v1/relationshipTypes/{relationshipTypeId}

    :reqjson string description: Relationship type description
    :reqjson object properties: Relationship type properties
    :reqjson string updateMask: Field mask

    :resjson string name: Relationship type resource name
    :resjson string description: Relationship type description
    :resjson object properties: Relationship type properties

..  http:example:: curl wget python-requests

    PATCH /v1/relationshipTypes/HAS_BRAND HTTP/1.1
    Host: data.api.exabel.com
    Accept: application/json
    X-Api-Key: API_KEY_GOES_HERE
    Content-Type: application/json; charset=utf-8

    {
      "description": "Denotes a company that owns a brand",
      "updateMask": "description"
    }


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "name": "relationshipTypes/HAS_BRAND",
      "description": "Denotes a company that owns a brand",
      "properties": {}
    }


Delete relationship type
------------------------

Delete is not supported by the API. If you need to delete a relationship type, contact support@exabel.com.


Relationships
*************

A `relationship` is a directed connection between two entities with a relationship type, that is, it
is a connection `from` one entity `to` another entity. There can only be a single relationship of
the same type and in the same direction between any two entities.

Typically a relationship only makes sense for specific entity types: For example, the relationship
HAS_LISTING requires the `from` entity to be a company and the `to` entity to be a listing. However,
this restriction is not enforced.

Exabel maintains a large number relationships between entities in the global namespace. These
relationship all have one of the relationship types in the global namespace.

Customers can create new relationships between any entities they have access to.

The collection id for relationships is ``relationships``.


List relationships
------------------

..  http:get:: /v1/relationshipTypes/{relationshipTypeId}/relationships

    :query fromEntity: The entity resource name of the start point of the relationship on the form
        ``entityTypes/{entityTypeId}}/entities/{entityId}``.
    :query toEntity: The entity resource name of the end point of the relationship on the form
        ``entityTypes/{entityTypeId}}/entities/{entityId}``.
    :query int pageSize: The maximum number of results to return. Defaults to 1000, which is also the maximum value
        of this field.
    :query string pageToken: The page token to resume the results from, as returned from a previous request to this
        method with the same query parameters.

    At least one of ``fromEntity`` and ``toEntity`` must be provided.

    Use ``-`` for ``relationshipTypeId`` to get relationships of all types.

    :resjson array relationships: The resulting relationships.
    :resjson string nextPageToken: The page token where the list continues. Can be sent to a subsequent query.
    :resjson int totalSize: The total number of results, irrespective of paging.

    To get *all* relationships between two entities, perform the request a second time with ``fromEntity`` and
    ``toEntity`` swapped.

..  http:example:: curl wget python-requests

    GET /v1/relationshipTypes/HAS_BRAND/relationships?fromEntity=entityTypes/company/entities/001yfz_e-volkswagen_ag HTTP/1.1
    Host: data.api.exabel.com
    Accept: application/json
    X-Api-Key: API_KEY_GOES_HERE


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "relationships":
        [
          {
            "parent": "relationshipTypes/HAS_BRAND",
            "fromEntity": "entityTypes/company/entities/001yfz_e-volkswagen_ag",
            "toEntity": "entityTypes/brand/entities/customer1.skoda"
          },
          {
            "parent": "relationshipTypes/HAS_BRAND",
            "fromEntity": "entityTypes/company/entities/001yfz_e-volkswagen_ag",
            "toEntity": "entityTypes/brand/entities/customer1.audi"
          },
          {
            "parent": "relationshipTypes/HAS_BRAND",
            "fromEntity": "entityTypes/company/entities/001yfz_e-volkswagen_ag",
            "toEntity": "entityTypes/brand/entities/customer1.vw"
          }
        ],
      "nextPageToken": "graph:entityTypes::brand:entities:customer1:vw",
      "totalSize": 3
    }


Get relationship
----------------

..  http:get:: /v1/relationshipTypes/{relationshipTypeId}/relationships

    :query fromEntity: The entity resource name of the start point of the relationship on the form
        ``entityTypes/{entityTypeId}}/entities/{entityId}`` (required).
    :query toEntity: The entity resource name of the end point of the relationship on the form
        ``entityTypes/{entityTypeId}}/entities/{entityId}`` (required).

    :resjson string parent: Relationship type resource name.
    :resjson string fromEntity: The entity resource name of the start point of the relationship.
    :resjson string toEntity: The entity resource name of the end point of the relationship.
    :resjson string description: Relationship description.
    :resjson boolean readOnly: Whether this resource is read only.
    :resjson object properties: Relationship properties.

..  http:example:: curl wget python-requests

    GET /v1/relationshipTypes/HAS_BRAND/relationships?fromEntity=entityTypes/company/entities/001yfz_e-volkswagen_ag&toEntity=entityTypes/brand/entities/customer1.skoda HTTP/1.1
    Host: data.api.exabel.com
    Accept: application/json
    X-Api-Key: API_KEY_GOES_HERE


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "parent": "relationshipTypes/HAS_BRAND",
      "fromEntity": "entityTypes/company/entities/001yfz_e-volkswagen_ag",
      "toEntity": "entityTypes/brand/entities/customer1.skoda",
      "description": "Škoda is a brand of Volkswagen AG",
      "properties": {}
    }



Create relationship
-------------------
..  http:post:: /v1/relationshipTypes/{relationshipTypeId}/relationships

    :reqjson string fromEntity: The entity resource name of the start point of the relationship (required).
    :reqjson string toEntity: The entity resource name of the end point of the relationship (required).
    :reqjson string description: Relationship description.
    :reqjson object properties: Relationship properties.

    :resjson string parent: Relationship type resource name.
    :resjson string fromEntity: The entity resource name of the start point of the relationship.
    :resjson string toEntity: The entity resource name of the end point of the relationship.
    :resjson string description: Relationship description.
    :resjson object properties: Relationship properties.

..  http:example:: curl wget python-requests

    POST /v1/relationshipTypes/HAS_BRAND/relationships HTTP/1.1
    Host: data.api.exabel.com
    Accept: application/json
    X-Api-Key: API_KEY_GOES_HERE
    Content-Type: application/json; charset=utf-8

    {
      "fromEntity": "entityTypes/company/entities/001yfz_e-volkswagen_ag",
      "toEntity": "entityTypes/brand/entities/customer1.skoda",
      "description": "Škoda is a brand of Volkswagen AG"
    }


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "parent": "relationshipTypes/HAS_BRAND",
      "fromEntity": "entityTypes/company/entities/001yfz_e-volkswagen_ag",
      "toEntity": "entityTypes/brand/entities/customer1.skoda",
      "description": "Škoda is a brand of Volkswagen AG",
      "properties": {}
    }


Update relationship
-------------------
..  http:patch:: /v1/relationshipTypes/{relationshipTypeId}/relationships

    :reqjson string fromEntity: The entity resource name of the start point of the relationship (required).
    :reqjson string toEntity: The entity resource name of the end point of the relationship (required).
    :reqjson string description: Relationship description.
    :reqjson object properties: Relationship properties.
    :reqjson string updateMask: Field mask.

    :resjson string parent: Relationship type resource name.
    :resjson string fromEntity: The entity resource name of the start point of the relationship.
    :resjson string toEntity: The entity resource name of the end point of the relationship.
    :resjson string description: Relationship description.
    :resjson object properties: Relationship properties.

..  http:example:: curl wget python-requests

    PATCH /v1/relationshipTypes/HAS_BRAND/relationships HTTP/1.1
    Host: data.api.exabel.com
    Accept: application/json
    X-Api-Key: API_KEY_GOES_HERE
    Content-Type: application/json; charset=utf-8

    {
      "fromEntity": "entityTypes/company/entities/001yfz_e-volkswagen_ag",
      "toEntity": "entityTypes/brand/entities/customer1.skoda",
      "description": "Škoda is a brand of Volkswagen AG",
      "properties": {
        "ownedSince": "1994-12-19"
      },
      "updateMask": "description,properties"
    }


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "parent": "relationshipTypes/HAS_BRAND",
      "fromEntity": "entityTypes/company/entities/001yfz_e-volkswagen_ag",
      "toEntity": "entityTypes/brand/entities/customer1.skoda",
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

    DELETE /v1/relationshipTypes/HAS_BRAND/relationships?fromEntity=entityTypes/company/entities/001yfz_e-volkswagen_ag&toEntity=entityTypes/brand/entities/customer1.skoda HTTP/1.1
    Host: data.api.exabel.com
    Accept: application/json
    X-Api-Key: API_KEY_GOES_HERE


    HTTP/1.1 200 OK
