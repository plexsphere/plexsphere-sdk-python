# plexsphere.BlueprintApi

All URIs are relative to *http://localhost*

Method | HTTP request | Description
------------- | ------------- | -------------
[**get_blueprint**](BlueprintApi.md#get_blueprint) | **GET** /v1/blueprints/{id} | Fetch a Blueprint by identifier.
[**list_blueprints**](BlueprintApi.md#list_blueprints) | **GET** /v1/blueprints | List the Blueprint Catalog.


# **get_blueprint**
> BlueprintResponse get_blueprint(id)

Fetch a Blueprint by identifier.

Returns the Blueprint identified by `{id}` together with its
ordered `versions` array. Each version carries a typed
`parameter_schema` — the closed-set parameter declarations an
operator fills in when provisioning a Resource from the
version.

The handler runs the `blueprint#user` ReBAC check BEFORE the
persistence read; an unauthorised caller therefore receives
`403` without the existence side-channel a "load-then-check"
flow would leak. A missing Blueprint surfaces as
`404 blueprint_not_found`.


### Example


```python
import plexsphere
from plexsphere.models.blueprint_response import BlueprintResponse
from plexsphere.rest import ApiException
from pprint import pprint

# Defining the host is optional and defaults to http://localhost
# See configuration.py for a list of all supported configuration parameters.
configuration = plexsphere.Configuration(
    host = "http://localhost"
)


# Enter a context with an instance of the API client
with plexsphere.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = plexsphere.BlueprintApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Blueprint identifier (UUIDv7). Bound on `/v1/blueprints/{id}` for the read-only Blueprint Catalog surface. 

    try:
        # Fetch a Blueprint by identifier.
        api_response = api_instance.get_blueprint(id)
        print("The response of BlueprintApi->get_blueprint:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling BlueprintApi->get_blueprint: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Blueprint identifier (UUIDv7). Bound on &#x60;/v1/blueprints/{id}&#x60; for the read-only Blueprint Catalog surface.  | 

### Return type

[**BlueprintResponse**](BlueprintResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Blueprint found. |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to read the addressed Blueprint. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Blueprint not found. Body is a &#x60;Problem&#x60; with &#x60;code: blueprint_not_found&#x60;.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **list_blueprints**
> BlueprintList list_blueprints(cursor=cursor, limit=limit)

List the Blueprint Catalog.

Returns a slug-ordered page of Blueprint Catalog entries the
caller is authorised to see. Per-row visibility is layered on
top of the page: rows whose `blueprint#user` ReBAC relation the
caller does not hold are filtered out, so the response items
are a subset of the persistence-level page.

The projection is catalogue metadata only — it carries each
Blueprint's identity, slug, display name, lifecycle status, and
timestamps, with an empty `versions` array. The per-version
manifests are never returned by the list surface; fetch a
single Blueprint to obtain its versions and typed parameter
schemas.

The pagination cursor is HMAC-signed and bound to the
per-(caller, pepper) pseudonym, so a cursor minted by one
principal cannot be replayed by another — the cross-caller
replay surfaces as `403 cursor_binding_mismatch`. A tampered
envelope or unknown version byte stays on `400 invalid_cursor`.


### Example


```python
import plexsphere
from plexsphere.models.blueprint_list import BlueprintList
from plexsphere.rest import ApiException
from pprint import pprint

# Defining the host is optional and defaults to http://localhost
# See configuration.py for a list of all supported configuration parameters.
configuration = plexsphere.Configuration(
    host = "http://localhost"
)


# Enter a context with an instance of the API client
with plexsphere.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = plexsphere.BlueprintApi(api_client)
    cursor = 'cursor_example' # str | Opaque continuation token returned by a previous call's `next_cursor`. The encoding is HMAC-signed by the server so a tampered cursor surfaces as `400`.  (optional)
    limit = 50 # int | Maximum number of items to return in a single page. The handler clamps the value to [1, 200] before forwarding it to the read service.  (optional) (default to 50)

    try:
        # List the Blueprint Catalog.
        api_response = api_instance.list_blueprints(cursor=cursor, limit=limit)
        print("The response of BlueprintApi->list_blueprints:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling BlueprintApi->list_blueprints: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **cursor** | **str**| Opaque continuation token returned by a previous call&#39;s &#x60;next_cursor&#x60;. The encoding is HMAC-signed by the server so a tampered cursor surfaces as &#x60;400&#x60;.  | [optional] 
 **limit** | **int**| Maximum number of items to return in a single page. The handler clamps the value to [1, 200] before forwarding it to the read service.  | [optional] [default to 50]

### Return type

[**BlueprintList**](BlueprintList.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Page of Blueprints. |  -  |
**400** | Invalid query parameters — typically a tampered or malformed cursor or an out-of-range &#x60;limit&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to list the Blueprint Catalog (body is a &#x60;PermissionDenied&#x60; problem) OR the pagination cursor was minted by a different caller and the per-(caller, pepper) HMAC binding rejected the replay (body is a &#x60;Problem&#x60; with &#x60;code &#x3D; cursor_binding_mismatch&#x60;).  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

