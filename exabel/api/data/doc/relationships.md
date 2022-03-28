---
title: Relationships
category: 61a1198a80c2b80010b68b81
slug: data-api-relationships
---
## Relationship types

Every relationship has a _relationship type_, such as `HAS_BRAND`, `LOCATED_IN` or `OWNED_BY`. The full resource name for a relationship type is `relationshipTypes/ns.NAME`.

The relationship types provided by Exabel are the following:

| Relationship type       | Resource name                               | Typical connected entities |
|-------------------------|---------------------------------------------|----------------------------|
| has listing             | `relationshipTypes/HAS_LISTING`             | regional → listing         |
| has primary regional    | `relationshipTypes/HAS_PRIMARY_REGIONAL`    | security → regional        |
| has regional            | `relationshipTypes/HAS_REGIONAL`            | security → regional        |
| has security            | `relationshipTypes/HAS_SECURITY`            | company → security         |
| located in              | `relationshipTypes/LOCATED_IN`              | company → country          |
| has security            | `relationshipTypes/HAS_SECURITY`            | company → security         |
| web domain owned by     | `relationshipTypes/WEB_DOMAIN_OWNED_BY`     | company → web domain       |
| has primary rbics       | `relationshipTypes/HAS_PRIMARY_RBICS`       | company → rbics            |
| has primary rbics focus | `relationshipTypes/HAS_PRIMARY_RBICS_FOCUS` | company → rbics            |

The relationship types provided by Exabel are read-only, meaning that customers cannot add new relationships with those relationship types. Customers are free to create new relationship types in their own namespace, using the API.

Some relationship types are marked as being an *ownership*. Relations of those types are aggregation paths in a data set, and they go from the owner to the sub entity.

The collection id for relationship types is `relationshipTypes`.

## Relationships

A _relationship_ is a directed connection between two entities with a relationship type, that is, it is a connection `from` one entity `to` another entity. There can only be a single relationship of the same type and in the same direction between any two entities.

Typically a relationship only makes sense for specific entity types: For example, the relationship `HAS_LISTING` requires the _from_ entity to be a company and the _to_ entity to be a listing. However, this restriction is not enforced.

Exabel maintains a large number relationships between entities in the global namespace. These relationships all have one of the relationship types in the global namespace.

Customers can create new relationships between any entities they have access to.

The collection id for relationships is `relationships`.

-[RelationshipService REST reference documentation](https://help.exabel.com/reference/relationshipservice)
-[RelationshipService gRPC reference documentation](https://help.exabel.com/docs/data-api-grpc-reference#relationshipservice)
