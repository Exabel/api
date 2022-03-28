---
title: Data API
category: 61a1198a80c2b80010b68b81
slug: data-api
---
The Exabel Data API can be used to upload custom data to the Exabel platform. Custom data may include _entities_, _relationships_ between entities, _signals_ and _time series_.

An entity represents a real world entity and Exabel maintains several sets of entities that can be used directly or be linked to custom entities. For example:
- Companies
- Securities
- Regionals
- Listings
- Exchanges
- Countries

Before uploading any data it is important to define a data model, including entities and relationships. If you only have company-level time series, you may not need any custom entities or relationships. However, if you have time series for other types of entities you may need to define a custom data model. You can also define a collection of signals to be a _data set_. The complete data set consists of those signals, entities having time series for those signals, and all entities found by repeatedly following _incoming_ relationships of _ownership_/_aggregation_ type.

Once the data model has been defined, entities, relationships and time series can be uploaded through the Data API. You can either use the REST endpoint directly or use the Exabel Python SDK found at https://pypi.org/project/exabel-data-sdk/.

Finally, once data has been uploaded, the data is available for use in the platform. See the [Exable DSL reference](https://doc.exabel.com/dsl/data/data_api_signals.html) for an in-depth description on how to use the data within the platform.

To access the Data API, you need an API key provided by Exabel.

You can access the Data API either directly by using gRPC or REST, or by using the [Exabel Python SDK](https://pypi.org/project/exabel-data-sdk/)

The Data API service definitions can be found on [GitHub](https://github.com/Exabel/api/tree/master/exabel/api/data/v1).
