
Signals
=======

A signal is a type of time series that is connected to exactly one entity type, for instance trade price for listings,
earnings for companies, traffic numbers for web domains.

The collection id for signals is ``signals``.


Get signal
----------

..  http:get:: /v1/signals/{signalId}

    :resjson string name: Signal resource name
    :resjson string entityType: Resource name of the entity type this signal may exist for
    :resjson string displayName: Signal display name
    :resjson string description: Signal description
    :resjson string downsamplingMethod: The default downsampling method to use when this signal is re-sampled into
        larger intervals. When two or more values in an interval needs to be aggregated into a single value, specifies
        how they are combined. One of ``MEAN``, ``FIRST``, ``LAST``, ``SUM``, ``MIN``, ``MAX``.

..  http:example:: curl wget python-requests

    GET /v1/signals/customer1.visitors HTTP/1.1
    Host: graph.api.exabel.com


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "name": "signals/customer1.visitors",
      "entityType": "entityTypes/customer1.stores",
      "displayName": "Daily visitors",
      "description": "The number of visitors in a store per day",
      "downsamplingMethod": "SUM"
    }


Create signal
-------------

..  http:post:: /v1/signals

    :reqjson string name: Signal resource name on the form ``signals/{signalId}`` (required)
    :reqjson string entityType: Resource name of the entity type this signal may exist for (required)
    :reqjson string displayName: Signal display name (required)
    :reqjson string description: Signal description
    :reqjson string downsamplingMethod: The default downsampling method to use when this signal is re-sampled into
        larger intervals. When two or more values in an interval needs to be aggregated into a single value, specifies
        how they are combined. One of ``MEAN``, ``FIRST``, ``LAST``, ``SUM``, ``MIN``, ``MAX``.

    :resjson string name: Signal resource name
    :resjson string entityType: Resource name of the entity type this signal may exist for
    :resjson string displayName: Signal display name
    :resjson string description: Signal description
    :resjson string downsamplingMethod: The default downsampling method to use when this signal is re-sampled into
        larger intervals. When two or more values in an interval needs to be aggregated into a single value, specifies
        how they are combined. One of ``MEAN``, ``FIRST``, ``LAST``, ``SUM``, ``MIN``, ``MAX``.

..  http:example:: curl wget python-requests

    POST /v1/signals HTTP/1.1
    Host: graph.api.exabel.com
    Content-Type: application/json; charset=utf-8

    {
      "name": "signals/customer1.visitors",
      "entityType": "entityTypes/customer1.stores",
      "displayName": "Daily visitors",
      "description": "The number of visitors in a store per day"
    }


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "name": "signals/customer1.visitors",
      "entityType": "entityTypes/customer1.stores",
      "displayName": "Daily visitors",
      "description": "The number of visitors in a store per day"
    }


Update signal
-------------

..  http:patch:: /v1/signals/{signalId}

    :reqjson string entityType: Resource name of the entity type this signal may exist for
    :reqjson string displayName: Signal display name
    :reqjson string description: Signal description
    :reqjson string downsamplingMethod: The default downsampling method to use when this signal is re-sampled into
        larger intervals. When two or more values in an interval needs to be aggregated into a single value, specifies
        how they are combined. One of ``MEAN``, ``FIRST``, ``LAST``, ``SUM``, ``MIN``, ``MAX``.
    :reqjson array updateMask: Field mask (required)


    :resjson string name: Signal resource name
    :resjson string entityType: Resource name of the entity type this signal may exist for
    :resjson string displayName: Signal display name
    :resjson string description: Signal description
    :resjson string downsamplingMethod: The default downsampling method to use when this signal is re-sampled into
        larger intervals. When two or more values in an interval needs to be aggregated into a single value, specifies
        how they are combined. One of ``MEAN``, ``FIRST``, ``LAST``, ``SUM``, ``MIN``, ``MAX``.

..  http:example:: curl wget python-requests

    PATCH /v1/signals/customer1.visitors HTTP/1.1
    Host: graph.api.exabel.com
    Content-Type: application/json; charset=utf-8

    {
      "entityType": "entityTypes/customer1.stores",
      "displayName": "Daily visitors",
      "description": "The number of visitors in a store per day",
      "updateMask": ["entityType", "displayName", "description"]
    }


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "name": "signals/customer1.visitors",
      "entityType": "entityTypes/customer1.stores",
      "displayName": "Daily visitors",
      "description": "The number of visitors in a store per day"
    }


Delete signal
-------------

..  note:: **All** time series for this signal will also be deleted!

..  http:delete:: /v1/signals/{signalId}

..  http:example:: curl wget python-requests

    DELETE /v1/signals/customer1.visitors HTTP/1.1
    Host: graph.api.exabel.com


    HTTP/1.1 200 OK
