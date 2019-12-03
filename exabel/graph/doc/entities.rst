
Entities
==========================================

Both entity type names and entity names must start with a letter, followup by letters, numbers, dot,
hyphen (minus sign), or underscore. Names must be 1 to 64 characters long. Letters are limited to ASCII
letters. Both uppercase and lowercase letters are allowed. Note that names are stored case sensitive,
i.e. "APPLE" is not equal to "Apple".

Entity types
************

Used to retrieve the entity type catalogue.

* List entity types: ``GET /v1/entityTypes``
* Get entity type details: ``GET /v1/entityTypes/{entityTypeName}``

Entities
********

* List: ``GET /v1/entityTypes/{entityTypeName}/entities``
* Get: ``GET /v1/entityTypes/{entityTypeName}/entities/{entityName}``
* Create:  ``POST /v1/entityTypes/{entityTypeName}/entities/{entityName}``
* Update:  ``PUT /v1/entityTypes/{entityTypeName}/entities/{entityName}``
