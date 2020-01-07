
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

..  http:get:: /v1/entityTypes

    :resjsonarr string name: Entity type resource name

..  http:example:: curl wget python-requests

    GET /v1/entityTypes HTTP/1.1
    Host: data.api.exabel.com


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    [
      {
        "name": "entityTypes/exabel.brand"
      },
      {
        "name": "entityTypes/exabel.region"
      },
      {
        "name": "entityTypes/customer1.factory"
      }
    ]


Get entity type details
-----------------------

..  http:get:: /v1/entityTypes/{entityTypeId}

    :resjson string name: Entity type resource name
    :resjson string displayName: Entity type display name
    :resjson string description: Entity type description

..  http:example:: curl wget python-requests

    GET /v1/entityTypes/exabel.brand HTTP/1.1
    Host: data.api.exabel.com


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "name": "entityTypes/exabel.brand",
      "displayName": "Brand",
      "description": "Brands owned by companies"
    }


Entities
********

An *entity* belongs to exactly one entity type and is usually a real-world instance of its type. They are created
and managed either by a customer or by Exabel, for instance Alphabet, Inc., www.amazon.com, Coca-Cola, EMEA.
Entities are the core concept of this API.

The collection id for entities is ``entities``.


List entities
-------------

Lists all entities of a given entity type.

..  http:get:: /v1/entityTypes/{entityTypeId}

    :resjsonarr string name: Entity resource name

..  http:example:: curl wget python-requests

    GET /v1/entityTypes/exabel.brand HTTP/1.1
    Host: data.api.exabel.com


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    [
      {
        "name": "entityTypes/exabel.brand/entity/customer1.audi"
      },
      {
        "name": "entityTypes/exabel.brand/entity/customer1.skoda"
      },
      {
        "name": "entityTypes/exabel.brand/entity/customer1.vw"
      }
    ]

Get entity
----------

..  http:get:: /v1/entityTypes/{entityTypeId}/entities/{entityId}

    :resjson string name: Entity resource name
    :resjson string displayName: Entity display name
    :resjson string description: Entity description
    :resjson object properties: Entity properties


..  http:example:: curl wget python-requests

    GET /v1/entityTypes/exabel.brand/entities/customer1.skoda HTTP/1.1
    Host: data.api.exabel.com


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

      {
        "name": "entityTypes/exabel.brand/entities/customer1.skoda",
        "displayName": "Škoda"
      }


Create entity
-------------

..  http:post:: /v1/entityTypes/{entityTypeId}/entities

    :reqjson string name: Entity resource name on the format ``entityTypes/{entityTypeId}/entities/{entityId}`` (required)
    :reqjson string displayName: Entity display name
    :reqjson string description: Entity description
    :reqjson object properties: Entity properties

    :resjson string name: Entity resource name
    :resjson string displayName: Entity display name
    :resjson string description: Entity description
    :resjson object properties: Entity properties

..  http:example:: curl wget python-requests

    POST /v1/entityTypes/exabel.brand/entities HTTP/1.1
    Host: data.api.exabel.com
    Content-Type: application/json; charset=utf-8

    {
      "name": "entityTypes/exabel.brand/entities/customer1.skoda",
      "displayName": "Škoda"
    }


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "name": "entityTypes/exabel.brand/entities/customer1.skoda",
      "displayName": "Škoda"
    }


Update entity
-------------

..  http:patch:: /v1/entityTypes/{entityTypeId}/entities/{entityId}

    :reqjson string displayName: Entity display name
    :reqjson string description: Entity description
    :reqjson object properties: Entity properties
    :reqjson array updateMask: Field mask (required)

    :resjson string name: Entity resource name
    :resjson string displayName: Entity display name
    :resjson string description: Entity description
    :resjson object properties: Entity properties


..  http:example:: curl wget python-requests

    PATCH /v1/entityTypes/exabel.brand/entities/customer1.skoda HTTP/1.1
    Host: data.api.exabel.com
    Content-Type: application/json; charset=utf-8

    {
      "description": "Simply clever",
      "properties": {
        "brandType": "car"
      },
      "updateMask": ["description", "properties"]
    }


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "name": "entityTypes/exabel.brand/entities/customer1.skoda",
      "displayName": "Škoda",
      "description": "Simply clever"
      "properties": {
        "brandType": "car"
      },
    }


Delete entity
-------------

..  note:: **All** relationships and time series for this entity will also be deleted!

..  http:delete:: /v1/entityTypes/{entityTypeId}/entities/{entityId}

..  http:example:: curl wget python-requests

    DELETE /v1/entityTypes/exabel.brand/entities/customer1.skoda HTTP/1.1
    Host: data.api.exabel.com


    HTTP/1.1 200 OK

