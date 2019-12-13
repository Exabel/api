
Entities
==========================================

Both entity type names and entity names must start with a letter, followup by letters, numbers, dot,
hyphen (minus sign), or underscore. Names must be 1 to 64 characters long. Letters are limited to ASCII
letters. Both uppercase and lowercase letters are allowed. Note that names are stored case sensitive,
i.e. "APPLE" is not equal to "Apple".

Entity types
************

Used to retrieve the entity type catalogue.

List entity types
-----------------

..  http:example:: curl wget python-requests

    GET /v1/entityTypes HTTP/1.1
    Host: graph.api.exabel.com


    HTTP/1.1 200 OK
    Content-Type: application/json

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
    Content-Type: application/json

    {
      "name": "entityTypes/brand"
    }


Entities
********

List entities
-------------
..  http:example:: curl wget python-requests

    GET /v1/entityTypes/brand/entities HTTP/1.1
    Host: graph.api.exabel.com


    HTTP/1.1 200 OK
    Content-Type: application/json

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
    Content-Type: application/json

      {
        "name": "entityTypes/brand/entities/skoda",
        "display_name": "Škoda"
      }


Create entity
-------------
..  http:example:: curl wget python-requests

    POST /v1/entityTypes/brand/entities/Foo HTTP/1.1
    Host: graph.api.exabel.com
    Content-Type: application/json

    {
      "name": "entityTypes/brand/entities/skoda",
      "display_name": "Škoda"
    }


    HTTP/1.1 200 OK
    Content-Type: application/json

    {
      "name": "entityTypes/brand/entities/skoda",
      "display_name": "Škoda"
    }


Update entity
-------------
..  http:example:: curl wget python-requests

    PUT /v1/entityTypes/brand/entities/Foo HTTP/1.1
    Host: graph.api.exabel.com
    Content-Type: application/json

    {
      "name": "entityTypes/brand/entities/skoda",
      "display_name": "Škoda"
    }


    HTTP/1.1 200 OK
    Content-Type: application/json

    {
      "name": "entityTypes/brand/entities/skoda",
      "display_name": "Škoda"
    }
