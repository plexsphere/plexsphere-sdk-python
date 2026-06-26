# plexsphere.BlueprintApi

All URIs are relative to *http://localhost*

Method | HTTP request | Description
------------- | ------------- | -------------
[**get_blueprint**](BlueprintApi.md#get_blueprint) | **GET** /v1/blueprints/{id} | Fetch a Blueprint by identifier.
[**list_blueprints**](BlueprintApi.md#list_blueprints) | **GET** /v1/blueprints | List the Blueprint Catalog.
[**publish_blueprint_version**](BlueprintApi.md#publish_blueprint_version) | **POST** /v1/blueprints/{id}/versions | Publish a Blueprint version.
[**register_blueprint**](BlueprintApi.md#register_blueprint) | **POST** /v1/blueprints | Register a Blueprint Catalog entry.


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

# **publish_blueprint_version**
> BlueprintVersionResponse publish_blueprint_version(id, blueprint_version_create_request)

Publish a Blueprint version.

Publishes an immutable version under an existing Blueprint. The
service validates the request BEFORE any persistence write: the
`provider_kinds` enum members, the `injection_strategy` enum, the
`parameter_schema` document, and the structural XRD/Composition
manifest pair are each checked, so a malformed payload never
appends a `BlueprintVersionPublished` outbox row.

The handler authorises the caller against the `publish`
permission on the addressed Blueprint (`blueprint#publish`)
BEFORE invoking the service. The registrar of a Blueprint holds
`owner`, which grants `publish`; the grant is written
asynchronously after registration, so a publish issued in the
same instant as the register may be refused until the tuple
propagates.

A missing parent Blueprint surfaces as `404 blueprint_not_found`;
a re-published `(blueprint, version)` pair surfaces as
`409 blueprint_version_exists`. On success the handler emits a
`blueprint.publish` audit row and the service appends a
`BlueprintVersionPublished` outbox event in the same transaction.


### Example


```python
import plexsphere
from plexsphere.models.blueprint_version_create_request import BlueprintVersionCreateRequest
from plexsphere.models.blueprint_version_response import BlueprintVersionResponse
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
    blueprint_version_create_request = {"version":"1.0.0","provider_kinds":["aws"],"injection_strategy":"helm-values","xrd":{"kind":"CompositeResourceDefinition"},"composition":{"kind":"Composition"},"parameter_schema":{"parameters":[{"name":"storage_gb","type":"integer","required":true}]}} # BlueprintVersionCreateRequest | 

    try:
        # Publish a Blueprint version.
        api_response = api_instance.publish_blueprint_version(id, blueprint_version_create_request)
        print("The response of BlueprintApi->publish_blueprint_version:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling BlueprintApi->publish_blueprint_version: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Blueprint identifier (UUIDv7). Bound on &#x60;/v1/blueprints/{id}&#x60; for the read-only Blueprint Catalog surface.  | 
 **blueprint_version_create_request** | [**BlueprintVersionCreateRequest**](BlueprintVersionCreateRequest.md)|  | 

### Return type

[**BlueprintVersionResponse**](BlueprintVersionResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**201** | Version published. |  * Location - Absolute path of the issued Session for the single-Session read.  <br>  |
**400** | The request was rejected before any write — a malformed &#x60;version&#x60;, an unknown &#x60;provider_kind&#x60;, an invalid &#x60;injection_strategy&#x60;, a structurally invalid &#x60;parameter_schema&#x60;, an invalid XRD/Composition manifest pair, an invalid path &#x60;{id}&#x60;, or an undecodable body. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;invalid_provider_kind&#x60;, &#x60;invalid_injection_strategy&#x60;, &#x60;invalid_parameter_schema&#x60;, &#x60;invalid_manifest&#x60;, &#x60;invalid_blueprint&#x60;, &#x60;invalid_blueprint_id&#x60;, &#x60;invalid_body&#x60; }.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to publish versions of the addressed Blueprint. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Parent Blueprint not found. Body is a &#x60;Problem&#x60; with &#x60;code: blueprint_not_found&#x60;.  |  -  |
**409** | Conflict — the &#x60;(blueprint, version)&#x60; pair already exists. Body is a &#x60;Problem&#x60; with &#x60;code: blueprint_version_exists&#x60;.  |  -  |
**413** | Request body exceeded the Blueprint authorship ceiling enforced by the handler.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **register_blueprint**
> BlueprintResponse register_blueprint(blueprint_create_request)

Register a Blueprint Catalog entry.

Registers a new Blueprint — the catalogue entry under which one
or more immutable versions are later published. The aggregate
enforces every catalogue invariant: kebab-case `slug`, non-empty
`display_name`. A freshly registered Blueprint always carries
`status: active`.

The handler authorises the caller against the platform-level
`manage` relation (`platform#manage`) BEFORE invoking the
service so an unauthorised caller never produces a
`BlueprintRegistered` outbox row. A duplicate slug surfaces as
`409 blueprint_slug_conflict`.

On success the handler emits a `blueprint.register` audit row and
the service appends a `BlueprintRegistered` outbox event in the
same transaction. The authz-sync consumer maps that event to a
`blueprint:<id>#owner@<registrar>` tuple so the registrar gains
`publish` on the new Blueprint — the grant is eventually
consistent (see the `blueprint` tag description).


### Example


```python
import plexsphere
from plexsphere.models.blueprint_create_request import BlueprintCreateRequest
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
    blueprint_create_request = {"slug":"managed-postgres","display_name":"Managed PostgreSQL","description":"A managed PostgreSQL database instance."} # BlueprintCreateRequest | 

    try:
        # Register a Blueprint Catalog entry.
        api_response = api_instance.register_blueprint(blueprint_create_request)
        print("The response of BlueprintApi->register_blueprint:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling BlueprintApi->register_blueprint: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **blueprint_create_request** | [**BlueprintCreateRequest**](BlueprintCreateRequest.md)|  | 

### Return type

[**BlueprintResponse**](BlueprintResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**201** | Blueprint registered. |  * Location - Absolute path of the issued Session for the single-Session read.  <br>  |
**400** | Aggregate rejected the body — malformed &#x60;slug&#x60;, empty &#x60;display_name&#x60;, or an undecodable request body. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;invalid_blueprint&#x60;, &#x60;invalid_body&#x60; }.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to register Blueprints. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**409** | Conflict — the proposed slug collides with a persisted Blueprint. Body is a &#x60;Problem&#x60; with &#x60;code: blueprint_slug_conflict&#x60;.  |  -  |
**413** | Request body exceeded the Blueprint authorship ceiling enforced by the handler.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

