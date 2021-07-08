.. _signals:

Signals
=======

A `signal` is a type of time series, such as `price` or `revenue`. A signal is used to create
a time series for that signal connected to an entity.

There are no signals in the global namespace. Customers can create signals in their own namespace.

The purpose of signals is to identify kinds of time series. When you have created a signal, you can
upload one time series for that signal for any entity.

The collection id for signals is ``signals``.


List signals
-----------------

Retrieves the signal catalogue.

..  http:get:: /v1/signals

    :query int pageSize: The maximum number of results to return. Defaults to 1000, which is also the maximum value
        of this field.
    :query string pageToken: The page token to resume the results from, as returned from a previous request to this
        method with the same query parameters.
    :resjson array signals: The resulting signals.
    :resjson string nextPageToken: The page token where the list continues. Can be sent to a subsequent query.
    :resjson int totalSize: The total number of results, irrespective of paging.

..  http:example:: curl wget python-requests

    GET /v1/signals HTTP/1.1
    Host: data.api.exabel.com
    Accept: application/json
    X-Api-Key: API_KEY_GOES_HERE


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "signals": [
        {
          "name": "signals/customer1.customer_amount",
          "displayName": "Amount per customer",
          "description": "The amount spent per customer in a store per day",
          "downsamplingMethod": "MEAN"
        },
        {
          "name": "signals/customer1.visitors",
          "displayName": "Daily visitors",
          "description": "The number of visitors in a store per day",
          "downsamplingMethod": "SUM"
        }
      ],
      "nextPageToken": "graph:signals:customer1:visitors",
      "totalSize": 2
    }


Get signal
----------

..  http:get:: /v1/signals/{signalId}

    :resjson string name: Signal resource name.
    :resjson string displayName: Signal display name.
    :resjson string description: Signal description.
    :resjson string downsamplingMethod: The default downsampling method to use when this signal is re-sampled into
        larger intervals. When two or more values in an interval needs to be aggregated into a single value, specifies
        how they are combined. One of ``MEAN``, ``FIRST``, ``LAST``, ``SUM``, ``MIN``, ``MAX``.
    :resjson array entityTypes: Resource names of the types of entities this signal has time series for.

..  http:example:: curl wget python-requests

    GET /v1/signals/customer1.visitors HTTP/1.1
    Host: data.api.exabel.com
    Accept: application/json
    X-Api-Key: API_KEY_GOES_HERE


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "name": "signals/customer1.visitors",
      "displayName": "Daily visitors",
      "description": "The number of visitors in a store per day",
      "downsamplingMethod": "SUM",
      "readOnly": false,
      "entityTypes": ["entityTypes/customer1.stores"]
    }


Create signal
-------------

..  http:post:: /v1/signals

    :query boolean createLibrarySignal: Set to true to add the signal to the library when created.
    :reqjson string name: Signal resource name on the form ``signals/{signalId}`` (required). The part of the signal id
        after the namespace must start with a letter, and can only consist of letters, numbers, and underscore (_),
        and be at most 64 characters, i.e. match the regex ``[a-zA-Z]\w{0,63}``.
    :reqjson string displayName: Signal display name (required).
    :reqjson string description: Signal description.
    :reqjson string downsamplingMethod: The default downsampling method to use when this signal is re-sampled into
        larger intervals (required). When two or more values in an interval needs to be aggregated into a single value, specifies
        how they are combined. One of ``MEAN``, ``FIRST``, ``LAST``, ``SUM``, ``MIN``, ``MAX``.

    :resjson string name: Signal resource name.
    :resjson string displayName: Signal display name.
    :resjson string description: Signal description.
    :resjson string downsamplingMethod: The default downsampling method to use when this signal is re-sampled into
        larger intervals. When two or more values in an interval needs to be aggregated into a single value, specifies
        how they are combined. One of ``MEAN``, ``FIRST``, ``LAST``, ``SUM``, ``MIN``, ``MAX``.

..  http:example:: curl wget python-requests

    POST /v1/signals HTTP/1.1
    Host: data.api.exabel.com
    Accept: application/json
    X-Api-Key: API_KEY_GOES_HERE
    Content-Type: application/json; charset=utf-8

    {
      "name": "signals/customer1.visitors",
      "displayName": "Daily visitors",
      "description": "The number of visitors in a store per day",
      "downsamplingMethod": "SUM"
    }


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "name": "signals/customer1.visitors",
      "displayName": "Daily visitors",
      "description": "The number of visitors in a store per day",
      "downsamplingMethod": "SUM"
    }


Update signal
-------------

..  http:patch:: /v1/signals/{signalId}

    :reqjson string displayName: Signal display name.
    :reqjson string description: Signal description.
    :reqjson string downsamplingMethod: The default downsampling method to use when this signal is re-sampled into
        larger intervals. When two or more values in an interval needs to be aggregated into a single value, specifies
        how they are combined. One of ``MEAN``, ``FIRST``, ``LAST``, ``SUM``, ``MIN``, ``MAX``.
    :reqjson array updateMask: Fields to update. If not specified, the update behaves as a *full* update,
                               overwriting all existing fields and properties..


    :resjson string name: Signal resource name.
    :resjson string displayName: Signal display name.
    :resjson string description: Signal description.
    :resjson string downsamplingMethod: The default downsampling method to use when this signal is re-sampled into
        larger intervals. When two or more values in an interval needs to be aggregated into a single value, specifies
        how they are combined. One of ``MEAN``, ``FIRST``, ``LAST``, ``SUM``, ``MIN``, ``MAX``.

..  http:example:: curl wget python-requests

    PATCH /v1/signals/customer1.visitors HTTP/1.1
    Host: data.api.exabel.com
    Accept: application/json
    X-Api-Key: API_KEY_GOES_HERE
    Content-Type: application/json; charset=utf-8

    {
      "displayName": "Daily visitors",
      "description": "The number of visitors in a store per day",
      "updateMask": "displayName,description"
    }


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
      "name": "signals/customer1.visitors",
      "displayName": "Daily visitors",
      "description": "The number of visitors in a store per day"
    }


Delete signal
-------------

..  note:: **All** time series for this signal will also be deleted!

..  http:delete:: /v1/signals/{signalId}

..  http:example:: curl wget python-requests

    DELETE /v1/signals/customer1.visitors HTTP/1.1
    Host: data.api.exabel.com
    Accept: application/json
    X-Api-Key: API_KEY_GOES_HERE


    HTTP/1.1 200 OK
