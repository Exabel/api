
Entities
========

Entity types
************

An *entity type* is a group of entities having similar real world meaning, for instance company, web
domain, brand, or region. Every entity has a single entity type.

The available entity types provided by Exabel are the following:

.. list-table:: Entity types
    :widths: 25 40 20
    :header-rows: 1

    * - Entity type
      - Resource name
      - Customers can create entities
    * - brand
      - ``entityTypes/brand``
      - yes
    * - company
      - ``entityTypes/company``
      - no
    * - country
      - ``entityTypes/country``
      - no
    * - listing
      - ``entityTypes/listing``
      - no
    * - regional
      - ``entityTypes/regional``
      - no
    * - security
      - ``entityTypes/security``
      - no
    * - web_domain
      - ``entityTypes/web_domain``
      - no

A regional is a group of listings (of the same security) with the same trading currency in the same
region. The other entity types should be self-explanatory.

It is not possible to create a new entity type through the API. If you need a new entity type,
please contact us at support@exabel.com.


The collection id for entity types is ``entityTypes``.


List entity types
-----------------

Retrieves the entity type catalogue.

..  http:get:: /v1/entityTypes

    :query int pageSize: The maximum number of results to return. Defaults to 1000, which is also the maximum value
        of this field.
    :query string pageToken: The page token to resume the results from, as returned from a previous request to this
        method with the same query parameters.
    :resjson array entityTypes: The resulting entity types.
    :resjson string nextPageToken: The page token where the list continues. Can be sent to a subsequent query.
    :resjson int totalSize: The total number of results, irrespective of paging.

..  http:example:: curl wget python-requests

    GET /v1/entityTypes HTTP/1.1
    Host: data.api.exabel.com
    Accept: application/json
    X-Api-Key: API_KEY_GOES_HERE


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "entityTypes": [
        {
          "name": "entityTypes/brand",
          "displayName": "Brands",
          "description": "Company brands",
          "readOnly": true
        },
        {
          "name": "entityTypes/region",
          "displayName": "Regions",
          "description": "Geographical regions",
          "readOnly": true
        },
        {
          "name": "entityTypes/customer1.factory",
          "displayName": "Factory",
          "description": "Factories"
        }
      ],
      "nextPageToken": "graph:entityTypes:customer1:factory",
      "totalSize": 3
    }


Get entity type details
-----------------------

..  http:get:: /v1/entityTypes/{entityTypeId}

    :resjson string name: Entity type resource name.
    :resjson string displayName: Entity type display name.
    :resjson string description: Entity type description.
    :resjson boolean readOnly: Whether this resource is read only.

..  http:example:: curl wget python-requests

    GET /v1/entityTypes/brand HTTP/1.1
    Host: data.api.exabel.com
    Accept: application/json
    X-Api-Key: API_KEY_GOES_HERE


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "name": "entityTypes/brand",
      "displayName": "Brand",
      "description": "Brands owned by companies",
      "readOnly": true
    }


Entities
********

An `entity` is an instance of any one of the entity types, such as a company or a brand. The full
resource name for an entity is ``entityTypes/ns.type/entities/ns.name``. For example,
the company entity referring to Apple, Inc. has the resource name
``entityTypes/company/entities/F_000C7F-E``. (Note that the identifier does not specify any
namespace since the entity belongs to the global namespace.)

A large number of entities are created and managed by Exabel. Those entities cover all publicly
listed companies on a large number of exchanges, along with the corresponding securities and
listings.

Of the built-in entity types all except for the *brand* entity type are `read-only`, meaning that
new entities can only be added by Exabel. Customers can add entities with the *brand* entity type,
and any entity type that has been created in their namespace.

The collection id for entities is ``entities``.


List entities
-------------

Lists all entities of a given entity type.

..  http:get:: /v1/entityTypes/{entityTypeId}/entities

    :query int pageSize: The maximum number of results to return. Defaults to 1000, which is also the maximum value
        of this field.
    :query string pageToken: The page token to resume the results from, as returned from a previous request to this
        method with the same query parameters.
    :resjson array entities: The resulting entities.
    :resjson string nextPageToken: The page token where the list continues. Can be sent to a subsequent query.
    :resjson int totalSize: The total number of results, irrespective of paging.

..  http:example:: curl wget python-requests

    GET /v1/entityTypes/brand HTTP/1.1
    Host: data.api.exabel.com
    Accept: application/json
    X-Api-Key: API_KEY_GOES_HERE


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "entities": [
        {
          "name": "entityTypes/brand/entities/audi",
          "displayName": "Audi",
          "readOnly": true,
          "properties": {}
        },
        {
          "name": "entityTypes/brand/entities/customer1.skoda",
          "displayName": "Škoda",
          "properties": {}
        },
        {
          "name": "entityTypes/brand/entities/customer1.vw",
          "displayName": "VW",
          "properties": {}
        }
      ],
      "nextPageToken": "graph:entityTypes:brand:entities:customer1.vw",
      "totalSize": 3
    }

Get entity
----------

..  http:get:: /v1/entityTypes/{entityTypeId}/entities/{entityId}

    :resjson string name: Entity resource name.
    :resjson string displayName: Entity display name.
    :resjson string description: Entity description.
    :resjson boolean readOnly: Whether this resource is read only.
    :resjson object properties: Entity properties.


..  http:example:: curl wget python-requests

    GET /v1/entityTypes/brand/entities/customer1.skoda HTTP/1.1
    Host: data.api.exabel.com
    Accept: application/json
    X-Api-Key: API_KEY_GOES_HERE


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

      {
        "name": "entityTypes/brand/entities/customer1.skoda",
        "displayName": "Škoda",
        "properties": {}
      }


Create entity
-------------

..  http:post:: /v1/entityTypes/{entityTypeId}/entities

    :reqjson string name: Entity resource name on the format ``entityTypes/{entityTypeId}/entities/{entityId}``
        (required).
    :reqjson string displayName: Entity display name.
    :reqjson string description: Entity description.
    :reqjson object properties: Entity properties.

    :resjson string name: Entity resource name.
    :resjson string displayName: Entity display name.
    :resjson string description: Entity description.
    :resjson object properties: Entity properties.

..  http:example:: curl wget python-requests

    POST /v1/entityTypes/brand/entities HTTP/1.1
    Host: data.api.exabel.com
    Accept: application/json
    X-Api-Key: API_KEY_GOES_HERE
    Content-Type: application/json; charset=utf-8

    {
      "name": "entityTypes/brand/entities/customer1.skoda",
      "displayName": "Škoda"
    }


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "name": "entityTypes/brand/entities/customer1.skoda",
      "displayName": "Škoda",
      "properties": {}
    }


Update entity
-------------

..  http:patch:: /v1/entityTypes/{entityTypeId}/entities/{entityId}

    :reqjson string displayName: Entity display name.
    :reqjson string description: Entity description.
    :reqjson object properties: Entity properties.
    :reqjson string updateMask: Field mask.

    :resjson string name: Entity resource name.
    :resjson string displayName: Entity display name.
    :resjson string description: Entity description.
    :resjson object properties: Entity properties.


..  http:example:: curl wget python-requests

    PATCH /v1/entityTypes/brand/entities/customer1.skoda HTTP/1.1
    Host: data.api.exabel.com
    Accept: application/json
    X-Api-Key: API_KEY_GOES_HERE
    Content-Type: application/json; charset=utf-8

    {
      "description": "Simply clever",
      "properties": {
        "brandType": "car"
      },
      "updateMask": "description,properties"
    }


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "name": "entityTypes/brand/entities/customer1.skoda",
      "displayName": "Škoda",
      "description": "Simply clever",
      "properties": {
        "brandType": "car"
      },
    }


Delete entity
-------------

..  note:: **All** relationships and time series for this entity will also be deleted!

..  http:delete:: /v1/entityTypes/{entityTypeId}/entities/{entityId}

..  http:example:: curl wget python-requests

    DELETE /v1/entityTypes/brand/entities/customer1.skoda HTTP/1.1
    Host: data.api.exabel.com
    Accept: application/json
    X-Api-Key: API_KEY_GOES_HERE


    HTTP/1.1 200 OK


Search for entities
-------------

Search for entities. We currently support search based on ISIN or both MIC and ticker.

..  http:post:: /v1/entityTypes/{entityTypeId}/entities:search

..  http:example:: curl wget python-requests

    POST /v1/entityTypes/company/entities:search HTTP/1.1
    Host: data.api.exabel.com
    Accept: application/json
    X-Api-Key: API_KEY_GOES_HERE

    {
      "parent": "entityTypes/company",
      "terms": [{
          "field": "mic",
          "query": "XNAS"
        },
        {
          "field": "ticker",
          "query": "AAPL"
        }]
    }

    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "name": "graph:entity::company::F_000C7F-E",
      "displayName": "Apple, Inc.",
      "description": "",
      "properties": {},
    }
