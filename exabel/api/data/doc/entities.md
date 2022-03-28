---
title: Entities
category: 61a1198a80c2b80010b68b81
slug: data-api-entities
---
## Entity types

An _entity type_ is a group of entities having similar real world meaning, for instance company, web domain, brand, or region. Every entity has a single entity type.

Some standard entity types are provided by Exabel:

| Entity type | Resource name            | Customers can create entities | Can be listed |
|-------------|--------------------------|-------------------------------|---------------|
| brand       | `entityTypes/brand`      | yes                           | yes           |
| company     | `entityTypes/company`    | no                            | no            |
| country     | `entityTypes/country`    | no                            | yes           |
| currency    | `entityTypes/curreny`    | no                            | yes           |
| listing     | `entityTypes/listing`    | no                            | no            |
| regional    | `entityTypes/regional`   | no                            | no            |
| security    | `entityTypes/security`   | no                            | no            |
| web_domain  | `entityTypes/web_domain` | no                            | yes           |
| rbics       | `entityTypes/rbics`      | no                            | yes           |

A regional is a group of listings (of the same security) with the same trading currency in the same region. The other entity types should be self-explanatory.

Some entity types are marked as _associative_, representing a [many-to-many relationship](https://en.wikipedia.org/wiki/Associative_entity>) in the Data API graph. Entities of associative entity types will normally not show in search results, but they act as connections in order to connect a signal to a pair of entities.

The collection id for entity types is `entityTypes`.

## Entities

An _entity_ is an instance of any one of the entity types, such as a company or a brand. The full resource name for an entity is `entityTypes/ns.type/entities/ns.name`. For example, the company entity referring to Apple, Inc. has the resource name `entityTypes/company/entities/F_000C7F-E`. (Note that the identifier does not specify any namespace since the entity belongs to the global namespace.)

A large number of entities are created and managed by Exabel. Those entities cover all publicly listed companies on a large number of exchanges, along with the corresponding securities and listings. Some of those entity types are too large to be listed, but they can be searched for.

Of the built-in entity types all except for the _brand_ entity type are _read-only_, meaning that new entities can only be added by Exabel. Customers can add entities with the _brand_ entity type, and any entity type that has been created in their namespace.

The collection id for entities is `entities`.

-[EntityService REST reference documentation](https://help.exabel.com/reference/entityservice)
-[EntityService gRPC reference documentation](https://help.exabel.com/docs/data-api-grpc-reference#entityservice)
