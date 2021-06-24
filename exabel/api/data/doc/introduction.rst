
Introduction
============

The Exabel Data API can be used to upload custom data to the Exabel platform. Custom data may include *entities*,
*relationships* between entities and *time series*.

*Entities* represent some real world entities and Exabel maintains several sets of
entities that can be used directly or linked to custom entities. For example:

* Companies
* Securities
* Regionals
* Listings
* Exchanges
* Countries

Before uploading any data it is important to define a data model, including entities and relationships. If you only
have company-level time series, you may not need any custom entities and relationships. However, if you have time
series for other types of entities you may need to define a custom data model.

Once the data model has been defined, entities, relationships and time series can be uploaded through the Data API.
You can either use the REST endpoint directly or use the Exabel Python SDK (https://pypi.org/project/exabel-data-sdk/).

Finally, once data has been uploaded, the data is available for use in the platform.
Visit https://doc.exabel.com/data/graph for an in-depth description on how to use the data.
