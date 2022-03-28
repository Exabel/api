---
title: Data API examples
category: 61a1198a80c2b80010b68b81
slug: data-api-examples
---
## Uploading daily time series for listed companies

In this example you will see how you can create a signal and upload data for a number of companies using the Python SDK.

Suppose that you have collected data on customer sentiment for a large number of companies. You want to upload the data to the Exabel platform so that you can use the data in a dashboard or in models.

### Create a signal and upload a time series

**Step 1.** Create the signal, which in this case is a signal for `customer sentiment`:

```python
client.signal_api.create_signal(
  Signal(
    name="signals/ns.customer_sentiment",
    display_name="Customer sentiment",
    description="Customers’ emotions with regard to the companies",
  )
)
```

This creates a new signal in your namespace (here called `ns`), which can be used to upload sentiment time series for any number of companies.

**Step 2.** Find the companies. We offer two ways to search for companies: by ISIN and by MIC and ticker.

```python
company = client.entity_api.search_for_entities(
  entity_type="entityTypes/company",
  mic="XNAS",
  ticker="AAPL"
)[0]
print(company.name)
# Prints 'entityTypes/company/entities/F_000C7F-E'
```

**Step 3.** Upload the time series. To do this you must create a `pandas.Series` with the data, and then create the time series with the time series api.

```python
name = f"signals/ns.customer_sentiment/{company.name}"
client.time_series_api.create_time_series(name=name, series=series)
```

### View uploaded time series

The time series can be viewed in Signal Explorer in the Exabel platform with the DSL expression `graph_signal('ns.customer_sentiment')`.

See [`graph_signal`](https://doc.exabel.com/dsl/data/data_api_signals.html#graph_signal) for an in-depth description of how to use the `graph_signal` function.


### Download the time series

After you have uploaded the time series, you may want to verify that the upload has been successful by downloading it again.

```python
name = f"signals/ns.customer_sentiment/{company.name}"
result = client.time_series_api.get_time_series(
    name=name,
    start=pandas.Timestamp("2020-01-01"),
    end=pandas.Timestamp("2020-12-31")
)
print(result)
```

### Add more data to a time series

In many cases you may want to add new data points to an existing time series. This can be done in the following way. (If the new series contains data points that already exist, they are overwritten.)

```python
name = f"signals/ns.customer_sentiment/{company.name}"
result = client.time_series_api.append_time_series_data(
  name=name,
  series=new_data_points)
)
```

### Uploading time series for associated entities (e.g. brands)

In this example we create brands for companies, create a signal associated with brands and then upload time series for the brands.

In order to do this we must create entities for the brands, connect them with companies and finally upload the time series.

### Create brands

The `brand` entity type exists in the global namespace, but unlike for the other entity types,  Exabel has not created brand entities. Thus, if you want to work with time series connected to brands, you will have to create the brands yourself. This is done in the following way.

```python
client.entity_api.create_entity(
  Entity(
    name="entityTypes/brand/entities/ns.SUPER",
    display_name="Super",
    description="Super Brand",
    properties={},
  ),
  entity_type="entityTypes/brand",
)
client.entity_api.create_entity(
  Entity(
    name="entityTypes/brand/entities/ns.DUPER",
    display_name="Duper",
    description="Duper Brand",
    properties={},
  ),
  entity_type="entityTypes/brand",
)
```

### Create relationship type

The brands we created in the previous steps are now singular entities in the graph, that are not connected to anything else. We want add relationships between brands and the corresponding companies. In order to do this, we must first create the `relationship type` which defines that relationship.

We choose to call the relationship `HAS_BRAND`, so that the from node has type company and the to node has type brand. (Note that the entity type restriction is not included in the relationship type definition, and it will not be enforced by the server.)

```python
client.relationship_api.create_relationship_type(
  RelationshipType(
    name="relationshipTypes/ns.HAS_BRAND",
    description="Relation between a company and a brand it owns",
    properties={},
  )
)
```

### Associate a company with the brands

We can now add relationships between companies and brands, using the HAS_BRAND relationship type.

```python
client.relationship_api.create_relationship(
  Relationship(
    relationship_type="relationshipTypes/ns.HAS_BRAND",
    from_entity=company.name,
    to_entity="entityTypes/brand/entities/ns.SUPER",
    description="Company brand",
    properties={},
  )
)
client.relationship_api.create_relationship(
  Relationship(
    relationship_type="relationshipTypes/ns.HAS_BRAND",
    from_entity=company.name,
    to_entity="entityTypes/brand/entities/ns.DUPER",
    description="Company brand",
    properties={},
  )
)
```

### Create signal for brands and upload time series

Then we create a brand signal, allowing us to upload time series associated with a single brand.

```python
signal = client.signal_api.create_signal(
  Signal(
    name="signals/ns.brand_sentiment",
    display_name="Customer sentiment",
    description="Customers’ emotions with regard to the brands",
    entity_type="entityTypes/brand",
  )
)
client.time_series_api.create_time_series(
  name="signals/ns.brand_sentiment/entityTypes/brand/entities/ns.SUPER",
  series=super_series,
)
client.time_series_api.create_time_series(
  name="signals/ns.brand_sentiment/entityTypes/brand/entities/ns.DUPER",
  series=duper_series,
)


self.client.time_series_api.create_time_series(
  name="signals/ns.brand_sentiment/entityTypes/brand/entities/ns.SUPER",
  series=super_data,
)
self.client.time_series_api.create_time_series(
  name="signals/ns.brand_sentiment/entityTypes/brand/entities/ns.DUPER",
  series=duper_data,
)
```

### View uploaded time series

The time series can be viewed in Signal Explorer in the Exabel platform with the DSL expression `graph_signal('ns.brand_sentiment', ['ns.HAS_BRAND'])`.

See [`graph_signal`](https://doc.exabel.com/dsl/data/data_api_signals.html#graph_signal) for an in-depth description.
