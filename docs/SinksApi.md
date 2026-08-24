# plexsphere.SinksApi

All URIs are relative to *http://localhost*

Method | HTTP request | Description
------------- | ------------- | -------------
[**create_sink**](SinksApi.md#create_sink) | **POST** /v1/domains/{id}/sinks | Declare a telemetry sink for a Domain.
[**create_telemetry_route**](SinksApi.md#create_telemetry_route) | **POST** /v1/projects/{id}/telemetry-routes | Create a Telemetry Route for a Project.
[**delete_sink**](SinksApi.md#delete_sink) | **DELETE** /v1/sinks/{id} | Delete a tenant sink.
[**delete_telemetry_route**](SinksApi.md#delete_telemetry_route) | **DELETE** /v1/telemetry-routes/{id} | Delete a Telemetry Route.
[**get_sink**](SinksApi.md#get_sink) | **GET** /v1/sinks/{id} | Read a single sink.
[**get_telemetry_route**](SinksApi.md#get_telemetry_route) | **GET** /v1/telemetry-routes/{id} | Read a single Telemetry Route.
[**grant_sink_enablement**](SinksApi.md#grant_sink_enablement) | **POST** /v1/sinks/{id}/sink-enablements | Grant a sink to a Project (owner push).
[**list_built_in_sinks**](SinksApi.md#list_built_in_sinks) | **GET** /v1/sinks/built-in | List the built-in sinks the platform operates.
[**list_domain_sinks**](SinksApi.md#list_domain_sinks) | **GET** /v1/domains/{id}/sinks | List the tenant sinks a Domain declares.
[**list_sink_enablements**](SinksApi.md#list_sink_enablements) | **GET** /v1/projects/{id}/sink-enablements | List the sink enablements owned by a Project.
[**list_telemetry_routes**](SinksApi.md#list_telemetry_routes) | **GET** /v1/projects/{id}/telemetry-routes | List the Telemetry Routes a Project holds.
[**request_sink_enablement**](SinksApi.md#request_sink_enablement) | **POST** /v1/projects/{id}/sink-enablements | Request a sink enablement for a Project.
[**revoke_sink_enablement**](SinksApi.md#revoke_sink_enablement) | **POST** /v1/sink-enablements/{id}/revoke | Revoke a sink enablement.
[**update_sink**](SinksApi.md#update_sink) | **PUT** /v1/sinks/{id} | Replace a tenant sink&#39;s mutable state.
[**update_telemetry_route**](SinksApi.md#update_telemetry_route) | **PUT** /v1/telemetry-routes/{id} | Replace a Telemetry Route&#39;s state.


# **create_sink**
> Sink create_sink(id, sink_create_request)

Declare a telemetry sink for a Domain.

Declares a tenant sink the addressed Domain's Projects may be
enabled on. The handler checks the `manage` ReBAC permission on
the addressed Domain before any write: a sink derives its own
`manage` from `parent->manage`, so the Domain check is the same
authority the created sink answers to afterwards.

Only the two forwarding protocols are tenant-declarable, `otlp`
and `syslog`. The platform's own backends (`mimir`, `loki`,
`siem`) exist as built-in sinks and are never authored here.

`id` is optional. A caller that stores connection material for
the sink mints the id first, derives the KV path from the owning
Domain and that id, writes the material there, then declares the
sink under the same id. Omitting `id` lets the server mint one.

`credential` carries KV coordinates only — the mount and,
optionally, the version. The path is always derived server-side
from the owning Domain and the sink id, so no caller can point a
sink at material belonging to another tenant.

The slug is unique per Domain; a duplicate is rejected with
`409 sink_slug_taken`.


### Example

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

```python
import plexsphere
from plexsphere.models.sink import Sink
from plexsphere.models.sink_create_request import SinkCreateRequest
from plexsphere.rest import ApiException
from pprint import pprint

# Defining the host is optional and defaults to http://localhost
# See configuration.py for a list of all supported configuration parameters.
configuration = plexsphere.Configuration(
    host = "http://localhost"
)

# The client must configure the authentication and authorization parameters
# in accordance with the API server security policy.
# Examples for each auth method are provided below, use the example that
# satisfies your auth use case.

# Configure Bearer authorization (JWT): operatorBearer
configuration = plexsphere.Configuration(
    access_token = os.environ["BEARER_TOKEN"]
)

# Configure API key authorization: sessionCookie
configuration.api_key['sessionCookie'] = os.environ["API_KEY"]

# Uncomment below to setup prefix (e.g. Bearer) for API key, if needed
# configuration.api_key_prefix['sessionCookie'] = 'Bearer'

# Enter a context with an instance of the API client
with plexsphere.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = plexsphere.SinksApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Domain identifier (UUIDv7). Bound on `/v1/domains/{id}` for the tenancy CRUD surface. 
    sink_create_request = {"slug":"central-otlp","display_name":"Central OTLP collector","sink_type":"otlp","endpoint":"https://otlp.example.com:4317","settings":{"dataset":"production"}} # SinkCreateRequest | 

    try:
        # Declare a telemetry sink for a Domain.
        api_response = api_instance.create_sink(id, sink_create_request)
        print("The response of SinksApi->create_sink:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling SinksApi->create_sink: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Domain identifier (UUIDv7). Bound on &#x60;/v1/domains/{id}&#x60; for the tenancy CRUD surface.  | 
 **sink_create_request** | [**SinkCreateRequest**](SinkCreateRequest.md)|  | 

### Return type

[**Sink**](Sink.md)

### Authorization

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**201** | The sink was declared. The body carries the stored sink.  |  * Location - Canonical read URL of the created route — &#x60;/v1/telemetry-routes/{id}&#x60;.  <br>  |
**400** | A malformed &#x60;{id}&#x60; or an unreadable body. The Problem body&#39;s &#x60;code&#x60; field is &#x60;invalid_domain_id&#x60; or &#x60;invalid_body&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the &#x60;manage&#x60; ReBAC permission on the addressed Domain.  |  -  |
**404** | The addressed Domain does not exist. The Problem body&#39;s &#x60;code&#x60; field is &#x60;domain_not_found&#x60;.  |  -  |
**409** | The write cannot be applied. The Problem body&#39;s &#x60;code&#x60; field is &#x60;sink_slug_taken&#x60; when the slug is already held by another sink of the Domain, or &#x60;sink_limit_reached&#x60; when the Domain already holds the maximum number of sinks.  |  -  |
**422** | The aggregate refused the body. The Problem body&#39;s &#x60;code&#x60; field is &#x60;sink_invalid&#x60; — a malformed slug, an oversized display name, a type no tenant may declare, an endpoint invalid for the stated type or naming a literal address inside the platform&#39;s own network, a CA bundle that carries no certificate, or a dataset on a type other than &#x60;otlp&#x60;.  |  -  |
**500** | Internal server error. |  -  |
**501** | The sink surface is not yet provisioned in this build, so log scrapers can alert on the deferred-wiring state. The Problem body&#39;s &#x60;code&#x60; field is &#x60;sinks_not_provisioned&#x60;.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **create_telemetry_route**
> TelemetryRoute create_telemetry_route(id, telemetry_route_request)

Create a Telemetry Route for a Project.

Creates a Telemetry Route that states where the Project addressed
by `{id}` sends one signal. The handler checks the Project's
`deploy` ReBAC permission before any write.

A route names exactly one signal (`metrics`, `logs`, or `audit`)
and at least one target sink, and it may narrow what it carries:
`severity_floor` orders log records and is stated on a `logs`
route only, `name_prefix` selects metric series and is stated on a
`metrics` route only. Stating either on another signal is refused
with `422 telemetry_route_invalid` rather than carried along and
ignored at delivery.

Every target is checked at authoring time, so a route that would
deliver nowhere is refused where it is written: a `sink_ids` entry
naming no sink is `422 route_sink_not_found`, a tenant sink the
Project holds no live approved enablement for is
`422 route_sink_not_usable`, and a signal the sink's type cannot
receive is `422 route_signal_not_accepted`. A built-in sink is
reachable without an enablement.

The number of routes a Project may hold is bounded, because the
delivery engine walks them for every buffered batch; exceeding it
is `409 route_limit_reached`.


### Example

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

```python
import plexsphere
from plexsphere.models.telemetry_route import TelemetryRoute
from plexsphere.models.telemetry_route_request import TelemetryRouteRequest
from plexsphere.rest import ApiException
from pprint import pprint

# Defining the host is optional and defaults to http://localhost
# See configuration.py for a list of all supported configuration parameters.
configuration = plexsphere.Configuration(
    host = "http://localhost"
)

# The client must configure the authentication and authorization parameters
# in accordance with the API server security policy.
# Examples for each auth method are provided below, use the example that
# satisfies your auth use case.

# Configure Bearer authorization (JWT): operatorBearer
configuration = plexsphere.Configuration(
    access_token = os.environ["BEARER_TOKEN"]
)

# Configure API key authorization: sessionCookie
configuration.api_key['sessionCookie'] = os.environ["API_KEY"]

# Uncomment below to setup prefix (e.g. Bearer) for API key, if needed
# configuration.api_key_prefix['sessionCookie'] = 'Bearer'

# Enter a context with an instance of the API client
with plexsphere.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = plexsphere.SinksApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Project identifier (UUIDv7). Bound on `/v1/projects/{id}` for the tenancy CRUD surface, on `/v1/projects/{id}/credentials` for the operator-facing OpenBao Credential Broker inventory list, on `/v1/projects/{id}/credential-assignments` and `/v1/projects/{id}/cloud-assignments` for the assignment request/list surfaces, on `/v1/projects/{id}/blueprints` for the project-scoped Blueprint offer list, and on `/v1/projects/{id}/sink-enablements` and `/v1/projects/{id}/telemetry-routes` for the sink-enablement and Telemetry Route surfaces. 
    telemetry_route_request = {"signal":"logs","severity_floor":"warning","sink_ids":["0190a8b8-a0c0-7a0a-8a0a-a0a0a0a0a0f1"]} # TelemetryRouteRequest | 

    try:
        # Create a Telemetry Route for a Project.
        api_response = api_instance.create_telemetry_route(id, telemetry_route_request)
        print("The response of SinksApi->create_telemetry_route:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling SinksApi->create_telemetry_route: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Project identifier (UUIDv7). Bound on &#x60;/v1/projects/{id}&#x60; for the tenancy CRUD surface, on &#x60;/v1/projects/{id}/credentials&#x60; for the operator-facing OpenBao Credential Broker inventory list, on &#x60;/v1/projects/{id}/credential-assignments&#x60; and &#x60;/v1/projects/{id}/cloud-assignments&#x60; for the assignment request/list surfaces, on &#x60;/v1/projects/{id}/blueprints&#x60; for the project-scoped Blueprint offer list, and on &#x60;/v1/projects/{id}/sink-enablements&#x60; and &#x60;/v1/projects/{id}/telemetry-routes&#x60; for the sink-enablement and Telemetry Route surfaces.  | 
 **telemetry_route_request** | [**TelemetryRouteRequest**](TelemetryRouteRequest.md)|  | 

### Return type

[**TelemetryRoute**](TelemetryRoute.md)

### Authorization

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**201** | The route was created. The body carries the stored route.  |  * Location - Canonical read URL of the created route — &#x60;/v1/telemetry-routes/{id}&#x60;.  <br>  |
**400** | A malformed &#x60;{id}&#x60; or an unreadable body. The Problem body&#39;s &#x60;code&#x60; field is &#x60;invalid_project_id&#x60; or &#x60;invalid_body&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the &#x60;deploy&#x60; ReBAC permission on the addressed Project.  |  -  |
**404** | The addressed Project does not exist. The Problem body&#39;s &#x60;code&#x60; field is &#x60;project_not_found&#x60;.  |  -  |
**409** | The Project already holds the maximum number of routes. The Problem body&#39;s &#x60;code&#x60; field is &#x60;route_limit_reached&#x60;.  |  -  |
**422** | Semantic rejection. The Problem body&#39;s &#x60;code&#x60; field is &#x60;telemetry_route_invalid&#x60; (an unknown signal, a predicate stated on the wrong signal, an oversized name prefix, an empty &#x60;sink_ids&#x60;, or a repeated target), &#x60;route_sink_not_found&#x60;, &#x60;route_sink_not_usable&#x60;, or &#x60;route_signal_not_accepted&#x60;.  |  -  |
**500** | Internal server error. |  -  |
**501** | The Telemetry Route surface is not yet provisioned in this build, so log scrapers can alert on the deferred-wiring state. The Problem body&#39;s &#x60;code&#x60; field is &#x60;telemetry_routes_not_provisioned&#x60;.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **delete_sink**
> delete_sink(id)

Delete a tenant sink.

Deletes the tenant sink addressed by `{id}`. The handler checks
the `manage` ReBAC permission on the sink before the delete.

A built-in sink is platform-provided and belongs to no caller, so
a delete addressing one is refused with `409 sink_conflict`.
Deleting an absent sink returns `404 sink_not_found`.


### Example

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

```python
import plexsphere
from plexsphere.rest import ApiException
from pprint import pprint

# Defining the host is optional and defaults to http://localhost
# See configuration.py for a list of all supported configuration parameters.
configuration = plexsphere.Configuration(
    host = "http://localhost"
)

# The client must configure the authentication and authorization parameters
# in accordance with the API server security policy.
# Examples for each auth method are provided below, use the example that
# satisfies your auth use case.

# Configure Bearer authorization (JWT): operatorBearer
configuration = plexsphere.Configuration(
    access_token = os.environ["BEARER_TOKEN"]
)

# Configure API key authorization: sessionCookie
configuration.api_key['sessionCookie'] = os.environ["API_KEY"]

# Uncomment below to setup prefix (e.g. Bearer) for API key, if needed
# configuration.api_key_prefix['sessionCookie'] = 'Bearer'

# Enter a context with an instance of the API client
with plexsphere.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = plexsphere.SinksApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Sink identifier (UUIDv7). Bound on `/v1/sinks/{id}` for the single-sink read, replace, and delete, and on `/v1/sinks/{id}/sink-enablements` for the owner-push grant. 

    try:
        # Delete a tenant sink.
        api_instance.delete_sink(id)
    except Exception as e:
        print("Exception when calling SinksApi->delete_sink: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Sink identifier (UUIDv7). Bound on &#x60;/v1/sinks/{id}&#x60; for the single-sink read, replace, and delete, and on &#x60;/v1/sinks/{id}/sink-enablements&#x60; for the owner-push grant.  | 

### Return type

void (empty response body)

### Authorization

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**204** | The sink was deleted. No body. |  -  |
**400** | A malformed &#x60;{id}&#x60;. The Problem body&#39;s &#x60;code&#x60; field is &#x60;invalid_sink_id&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the &#x60;manage&#x60; ReBAC permission on the addressed sink.  |  -  |
**404** | No sink with the id exists. The Problem body&#39;s &#x60;code&#x60; field is &#x60;sink_not_found&#x60;.  |  -  |
**409** | The addressed sink is built in and is not deletable. The Problem body&#39;s &#x60;code&#x60; field is &#x60;sink_conflict&#x60;.  |  -  |
**500** | Internal server error. |  -  |
**501** | The sink surface is not yet provisioned in this build, so log scrapers can alert on the deferred-wiring state. The Problem body&#39;s &#x60;code&#x60; field is &#x60;sinks_not_provisioned&#x60;.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **delete_telemetry_route**
> delete_telemetry_route(id)

Delete a Telemetry Route.

Deletes the Telemetry Route addressed by `{id}`. The handler
resolves the route to learn its Project, then checks that
Project's `deploy` ReBAC permission before the delete. Deleting an
absent route returns `404 telemetry_route_not_found`.

A signal the Project routes nowhere falls back to the platform's
own backends, so deleting the last route for a signal does not
stop delivery.


### Example

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

```python
import plexsphere
from plexsphere.rest import ApiException
from pprint import pprint

# Defining the host is optional and defaults to http://localhost
# See configuration.py for a list of all supported configuration parameters.
configuration = plexsphere.Configuration(
    host = "http://localhost"
)

# The client must configure the authentication and authorization parameters
# in accordance with the API server security policy.
# Examples for each auth method are provided below, use the example that
# satisfies your auth use case.

# Configure Bearer authorization (JWT): operatorBearer
configuration = plexsphere.Configuration(
    access_token = os.environ["BEARER_TOKEN"]
)

# Configure API key authorization: sessionCookie
configuration.api_key['sessionCookie'] = os.environ["API_KEY"]

# Uncomment below to setup prefix (e.g. Bearer) for API key, if needed
# configuration.api_key_prefix['sessionCookie'] = 'Bearer'

# Enter a context with an instance of the API client
with plexsphere.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = plexsphere.SinksApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Telemetry Route identifier (UUIDv7). Bound on `/v1/telemetry-routes/{id}` for the single-route read, replace, and delete. 

    try:
        # Delete a Telemetry Route.
        api_instance.delete_telemetry_route(id)
    except Exception as e:
        print("Exception when calling SinksApi->delete_telemetry_route: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Telemetry Route identifier (UUIDv7). Bound on &#x60;/v1/telemetry-routes/{id}&#x60; for the single-route read, replace, and delete.  | 

### Return type

void (empty response body)

### Authorization

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**204** | The route was deleted. No body. |  -  |
**400** | A malformed &#x60;{id}&#x60;. The Problem body&#39;s &#x60;code&#x60; field is &#x60;invalid_telemetry_route_id&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the &#x60;deploy&#x60; ReBAC permission on the Project the route belongs to.  |  -  |
**404** | No route with the id exists. The Problem body&#39;s &#x60;code&#x60; field is &#x60;telemetry_route_not_found&#x60;.  |  -  |
**500** | Internal server error. |  -  |
**501** | The Telemetry Route surface is not yet provisioned in this build, so log scrapers can alert on the deferred-wiring state. The Problem body&#39;s &#x60;code&#x60; field is &#x60;telemetry_routes_not_provisioned&#x60;.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **get_sink**
> Sink get_sink(id)

Read a single sink.

Returns the sink addressed by `{id}`, tenant-declared or built-in.
The read is gated by the `observe` ReBAC permission on the sink,
which resolves for the sink's owner, an assigner, a Domain
manager, and a reader of a Project that holds a live grant.

The projection carries the sink's credential coordinates — the KV
mount, version, and derived path — and never the material stored
there.


### Example

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

```python
import plexsphere
from plexsphere.models.sink import Sink
from plexsphere.rest import ApiException
from pprint import pprint

# Defining the host is optional and defaults to http://localhost
# See configuration.py for a list of all supported configuration parameters.
configuration = plexsphere.Configuration(
    host = "http://localhost"
)

# The client must configure the authentication and authorization parameters
# in accordance with the API server security policy.
# Examples for each auth method are provided below, use the example that
# satisfies your auth use case.

# Configure Bearer authorization (JWT): operatorBearer
configuration = plexsphere.Configuration(
    access_token = os.environ["BEARER_TOKEN"]
)

# Configure API key authorization: sessionCookie
configuration.api_key['sessionCookie'] = os.environ["API_KEY"]

# Uncomment below to setup prefix (e.g. Bearer) for API key, if needed
# configuration.api_key_prefix['sessionCookie'] = 'Bearer'

# Enter a context with an instance of the API client
with plexsphere.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = plexsphere.SinksApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Sink identifier (UUIDv7). Bound on `/v1/sinks/{id}` for the single-sink read, replace, and delete, and on `/v1/sinks/{id}/sink-enablements` for the owner-push grant. 

    try:
        # Read a single sink.
        api_response = api_instance.get_sink(id)
        print("The response of SinksApi->get_sink:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling SinksApi->get_sink: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Sink identifier (UUIDv7). Bound on &#x60;/v1/sinks/{id}&#x60; for the single-sink read, replace, and delete, and on &#x60;/v1/sinks/{id}/sink-enablements&#x60; for the owner-push grant.  | 

### Return type

[**Sink**](Sink.md)

### Authorization

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | The stored sink. |  -  |
**400** | A malformed &#x60;{id}&#x60;. The Problem body&#39;s &#x60;code&#x60; field is &#x60;invalid_sink_id&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the &#x60;observe&#x60; ReBAC permission on the addressed sink.  |  -  |
**404** | No sink with the id exists. The Problem body&#39;s &#x60;code&#x60; field is &#x60;sink_not_found&#x60;.  |  -  |
**500** | Internal server error. |  -  |
**501** | The sink surface is not yet provisioned in this build, so log scrapers can alert on the deferred-wiring state. The Problem body&#39;s &#x60;code&#x60; field is &#x60;sinks_not_provisioned&#x60;.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **get_telemetry_route**
> TelemetryRoute get_telemetry_route(id)

Read a single Telemetry Route.

Returns the Telemetry Route addressed by `{id}`. The handler
resolves the route to learn the Project it belongs to, then checks
that Project's `read` ReBAC permission before returning it.


### Example

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

```python
import plexsphere
from plexsphere.models.telemetry_route import TelemetryRoute
from plexsphere.rest import ApiException
from pprint import pprint

# Defining the host is optional and defaults to http://localhost
# See configuration.py for a list of all supported configuration parameters.
configuration = plexsphere.Configuration(
    host = "http://localhost"
)

# The client must configure the authentication and authorization parameters
# in accordance with the API server security policy.
# Examples for each auth method are provided below, use the example that
# satisfies your auth use case.

# Configure Bearer authorization (JWT): operatorBearer
configuration = plexsphere.Configuration(
    access_token = os.environ["BEARER_TOKEN"]
)

# Configure API key authorization: sessionCookie
configuration.api_key['sessionCookie'] = os.environ["API_KEY"]

# Uncomment below to setup prefix (e.g. Bearer) for API key, if needed
# configuration.api_key_prefix['sessionCookie'] = 'Bearer'

# Enter a context with an instance of the API client
with plexsphere.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = plexsphere.SinksApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Telemetry Route identifier (UUIDv7). Bound on `/v1/telemetry-routes/{id}` for the single-route read, replace, and delete. 

    try:
        # Read a single Telemetry Route.
        api_response = api_instance.get_telemetry_route(id)
        print("The response of SinksApi->get_telemetry_route:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling SinksApi->get_telemetry_route: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Telemetry Route identifier (UUIDv7). Bound on &#x60;/v1/telemetry-routes/{id}&#x60; for the single-route read, replace, and delete.  | 

### Return type

[**TelemetryRoute**](TelemetryRoute.md)

### Authorization

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | The stored Telemetry Route. |  -  |
**400** | A malformed &#x60;{id}&#x60;. The Problem body&#39;s &#x60;code&#x60; field is &#x60;invalid_telemetry_route_id&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the &#x60;read&#x60; ReBAC permission on the Project the route belongs to.  |  -  |
**404** | No route with the id exists. The Problem body&#39;s &#x60;code&#x60; field is &#x60;telemetry_route_not_found&#x60;.  |  -  |
**500** | Internal server error. |  -  |
**501** | The Telemetry Route surface is not yet provisioned in this build, so log scrapers can alert on the deferred-wiring state. The Problem body&#39;s &#x60;code&#x60; field is &#x60;telemetry_routes_not_provisioned&#x60;.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **grant_sink_enablement**
> SinkEnablement grant_sink_enablement(id, sink_enablement_grant_body)

Grant a sink to a Project (owner push).

Enables the sink addressed by `{id}` in the Project named in the
body as a single authoritative action. The application service
checks the sink's `assign` permission before the write, then
creates the enablement already approved, writes the `uses` edge
that makes it effective, and appends a `SinkEnablementGranted`
outbox event.

This is the owner-push counterpart to `RequestSinkEnablement`,
which the consuming Project files and a second party decides. The
grant bypasses the four-eyes rule by design: there is no separate
requester to compare against, which is also why it exists —
requesting and then approving one's own request is refused, so
without this route a sink's owner could not place it at all.

A `sync_pending: true` in the response means the grant COMMITTED —
the row moved and its event is durable — but the arm that mirrors
it into the authorization graph has not completed, so the grant is
not yet effective. The direction is fail-safe and the caller must
NOT retry: a retry is refused by the live-unique index and would
report a conflict for work that already happened. The committed
event drives the same mutation through the authz-sync arm.

The same guards as the request path apply: a built-in sink is
refused with `422 sink_not_enableable`, a Project outside the
sink's Domain with `422 sink_domain_mismatch`, and a second live
enablement for the pair with
`409 duplicate_live_sink_enablement`.


### Example

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

```python
import plexsphere
from plexsphere.models.sink_enablement import SinkEnablement
from plexsphere.models.sink_enablement_grant_body import SinkEnablementGrantBody
from plexsphere.rest import ApiException
from pprint import pprint

# Defining the host is optional and defaults to http://localhost
# See configuration.py for a list of all supported configuration parameters.
configuration = plexsphere.Configuration(
    host = "http://localhost"
)

# The client must configure the authentication and authorization parameters
# in accordance with the API server security policy.
# Examples for each auth method are provided below, use the example that
# satisfies your auth use case.

# Configure Bearer authorization (JWT): operatorBearer
configuration = plexsphere.Configuration(
    access_token = os.environ["BEARER_TOKEN"]
)

# Configure API key authorization: sessionCookie
configuration.api_key['sessionCookie'] = os.environ["API_KEY"]

# Uncomment below to setup prefix (e.g. Bearer) for API key, if needed
# configuration.api_key_prefix['sessionCookie'] = 'Bearer'

# Enter a context with an instance of the API client
with plexsphere.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = plexsphere.SinksApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Sink identifier (UUIDv7). Bound on `/v1/sinks/{id}` for the single-sink read, replace, and delete, and on `/v1/sinks/{id}/sink-enablements` for the owner-push grant. 
    sink_enablement_grant_body = {"project_id":"0190a8b8-a0c0-7a0a-8a0a-a0a0a0a0a0c1"} # SinkEnablementGrantBody | 

    try:
        # Grant a sink to a Project (owner push).
        api_response = api_instance.grant_sink_enablement(id, sink_enablement_grant_body)
        print("The response of SinksApi->grant_sink_enablement:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling SinksApi->grant_sink_enablement: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Sink identifier (UUIDv7). Bound on &#x60;/v1/sinks/{id}&#x60; for the single-sink read, replace, and delete, and on &#x60;/v1/sinks/{id}/sink-enablements&#x60; for the owner-push grant.  | 
 **sink_enablement_grant_body** | [**SinkEnablementGrantBody**](SinkEnablementGrantBody.md)|  | 

### Return type

[**SinkEnablement**](SinkEnablement.md)

### Authorization

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**201** | The enablement was granted, already approved. No &#x60;Location&#x60; header is sent — the aggregate has no by-id read surface; poll it through the Project&#39;s enablement list. A &#x60;sync_pending&#x60; of &#x60;true&#x60; means the grant is committed but not yet effective.  |  -  |
**400** | A malformed &#x60;{id}&#x60; or an unreadable body. The Problem body&#39;s &#x60;code&#x60; field is &#x60;invalid_sink_id&#x60;, &#x60;invalid_body&#x60;, or &#x60;invalid_project_id&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the &#x60;assign&#x60; ReBAC permission on the addressed sink.  |  -  |
**404** | The addressed sink or the named Project does not exist. The Problem body&#39;s &#x60;code&#x60; field is &#x60;sink_not_found&#x60; or &#x60;project_not_found&#x60;.  |  -  |
**409** | A live enablement already exists for the same (Project, sink) pair. The Problem body&#39;s &#x60;code&#x60; field is &#x60;duplicate_live_sink_enablement&#x60;.  |  -  |
**422** | Semantic rejection. The Problem body&#39;s &#x60;code&#x60; field is &#x60;sink_not_enableable&#x60; when the addressed sink is built in, or &#x60;sink_domain_mismatch&#x60; when the named Project belongs to a Domain other than the sink&#39;s.  |  -  |
**500** | Internal server error. |  -  |
**501** | The sink-enablement surface is not yet provisioned in this build, so log scrapers can alert on the deferred-wiring state. The Problem body&#39;s &#x60;code&#x60; field is &#x60;sink_enablements_not_provisioned&#x60;.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **list_built_in_sinks**
> SinkList list_built_in_sinks()

List the built-in sinks the platform operates.

Returns the platform-provided sinks, ordered by slug. A built-in
sink names one of the platform's own backends, carries no Domain,
no endpoint, no credential and no TLS posture, and is addressed by
the slug that equals its type. A Telemetry Route reaches a built-in
sink without an enablement.

The read requires an authenticated principal and nothing more.
Built-in sinks are platform fixtures: they carry no authorization
tuples at all, so there is no object to check a relation against,
and they are legitimate route targets for every Project, so every
authenticated principal may list them. The roster names the
backends and their ids; it discloses no tenant state.


### Example

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

```python
import plexsphere
from plexsphere.models.sink_list import SinkList
from plexsphere.rest import ApiException
from pprint import pprint

# Defining the host is optional and defaults to http://localhost
# See configuration.py for a list of all supported configuration parameters.
configuration = plexsphere.Configuration(
    host = "http://localhost"
)

# The client must configure the authentication and authorization parameters
# in accordance with the API server security policy.
# Examples for each auth method are provided below, use the example that
# satisfies your auth use case.

# Configure Bearer authorization (JWT): operatorBearer
configuration = plexsphere.Configuration(
    access_token = os.environ["BEARER_TOKEN"]
)

# Configure API key authorization: sessionCookie
configuration.api_key['sessionCookie'] = os.environ["API_KEY"]

# Uncomment below to setup prefix (e.g. Bearer) for API key, if needed
# configuration.api_key_prefix['sessionCookie'] = 'Bearer'

# Enter a context with an instance of the API client
with plexsphere.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = plexsphere.SinksApi(api_client)

    try:
        # List the built-in sinks the platform operates.
        api_response = api_instance.list_built_in_sinks()
        print("The response of SinksApi->list_built_in_sinks:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling SinksApi->list_built_in_sinks: %s\n" % e)
```



### Parameters

This endpoint does not need any parameter.

### Return type

[**SinkList**](SinkList.md)

### Authorization

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | The built-in sinks the platform operates. |  -  |
**401** | Caller is not authenticated. |  -  |
**500** | Internal server error. |  -  |
**501** | The sink surface is not yet provisioned in this build, so log scrapers can alert on the deferred-wiring state. The Problem body&#39;s &#x60;code&#x60; field is &#x60;sinks_not_provisioned&#x60;.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **list_domain_sinks**
> SinkList list_domain_sinks(id)

List the tenant sinks a Domain declares.

Returns the tenant sinks declared by the addressed Domain,
ordered by slug. Built-in sinks belong to no Domain and are read
through `GET /v1/sinks/built-in` instead.

The read is gated by the `read` ReBAC permission on the addressed
Domain, checked before the persistence read. Every sink in the
list belongs to the one path Domain, so that check authorises the
whole list and no per-row filter runs. This is the Domain's read
surface for its destinations; declaring, updating, and deleting a
sink stay on `manage`.

The projection carries each sink's credential coordinates — the
KV mount, version, and derived path — and never the material
stored there.


### Example

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

```python
import plexsphere
from plexsphere.models.sink_list import SinkList
from plexsphere.rest import ApiException
from pprint import pprint

# Defining the host is optional and defaults to http://localhost
# See configuration.py for a list of all supported configuration parameters.
configuration = plexsphere.Configuration(
    host = "http://localhost"
)

# The client must configure the authentication and authorization parameters
# in accordance with the API server security policy.
# Examples for each auth method are provided below, use the example that
# satisfies your auth use case.

# Configure Bearer authorization (JWT): operatorBearer
configuration = plexsphere.Configuration(
    access_token = os.environ["BEARER_TOKEN"]
)

# Configure API key authorization: sessionCookie
configuration.api_key['sessionCookie'] = os.environ["API_KEY"]

# Uncomment below to setup prefix (e.g. Bearer) for API key, if needed
# configuration.api_key_prefix['sessionCookie'] = 'Bearer'

# Enter a context with an instance of the API client
with plexsphere.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = plexsphere.SinksApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Domain identifier (UUIDv7). Bound on `/v1/domains/{id}` for the tenancy CRUD surface. 

    try:
        # List the tenant sinks a Domain declares.
        api_response = api_instance.list_domain_sinks(id)
        print("The response of SinksApi->list_domain_sinks:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling SinksApi->list_domain_sinks: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Domain identifier (UUIDv7). Bound on &#x60;/v1/domains/{id}&#x60; for the tenancy CRUD surface.  | 

### Return type

[**SinkList**](SinkList.md)

### Authorization

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | The tenant sinks the Domain declares. |  -  |
**400** | A malformed &#x60;{id}&#x60;. The Problem body&#39;s &#x60;code&#x60; field is &#x60;invalid_domain_id&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the &#x60;read&#x60; ReBAC permission on the addressed Domain.  |  -  |
**404** | The addressed Domain does not exist. The Problem body&#39;s &#x60;code&#x60; field is &#x60;domain_not_found&#x60;.  |  -  |
**500** | Internal server error. |  -  |
**501** | The sink surface is not yet provisioned in this build, so log scrapers can alert on the deferred-wiring state. The Problem body&#39;s &#x60;code&#x60; field is &#x60;sinks_not_provisioned&#x60;.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **list_sink_enablements**
> SinkEnablementPage list_sink_enablements(id, cursor=cursor, limit=limit)

List the sink enablements owned by a Project.

Returns a creation-ordered page of the sink enablements filed for
the Project addressed by `{id}`, in every lifecycle state. The
application service checks the Project's `read` permission before
the persistence read; every row in the page belongs to the one
path Project, so that check authorises the whole page and no
per-row filter runs.

The pagination cursor is HMAC-signed and bound to the
per-(caller, pepper) pseudonym, so a cursor minted by one
principal cannot be replayed by another — the cross-caller replay
surfaces as `403 cursor_binding_mismatch`. A tampered envelope or
unknown version byte stays on `400 invalid_cursor`.


### Example

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

```python
import plexsphere
from plexsphere.models.sink_enablement_page import SinkEnablementPage
from plexsphere.rest import ApiException
from pprint import pprint

# Defining the host is optional and defaults to http://localhost
# See configuration.py for a list of all supported configuration parameters.
configuration = plexsphere.Configuration(
    host = "http://localhost"
)

# The client must configure the authentication and authorization parameters
# in accordance with the API server security policy.
# Examples for each auth method are provided below, use the example that
# satisfies your auth use case.

# Configure Bearer authorization (JWT): operatorBearer
configuration = plexsphere.Configuration(
    access_token = os.environ["BEARER_TOKEN"]
)

# Configure API key authorization: sessionCookie
configuration.api_key['sessionCookie'] = os.environ["API_KEY"]

# Uncomment below to setup prefix (e.g. Bearer) for API key, if needed
# configuration.api_key_prefix['sessionCookie'] = 'Bearer'

# Enter a context with an instance of the API client
with plexsphere.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = plexsphere.SinksApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Project identifier (UUIDv7). Bound on `/v1/projects/{id}` for the tenancy CRUD surface, on `/v1/projects/{id}/credentials` for the operator-facing OpenBao Credential Broker inventory list, on `/v1/projects/{id}/credential-assignments` and `/v1/projects/{id}/cloud-assignments` for the assignment request/list surfaces, on `/v1/projects/{id}/blueprints` for the project-scoped Blueprint offer list, and on `/v1/projects/{id}/sink-enablements` and `/v1/projects/{id}/telemetry-routes` for the sink-enablement and Telemetry Route surfaces. 
    cursor = 'cursor_example' # str | Opaque continuation token returned by a previous call's `next_cursor`. The encoding is HMAC-signed by the server so a tampered cursor surfaces as `400`.  (optional)
    limit = 50 # int | Maximum number of items to return in a single page. A value outside [1, 200] is rejected with a `400` Problem rather than silently clamped.  (optional) (default to 50)

    try:
        # List the sink enablements owned by a Project.
        api_response = api_instance.list_sink_enablements(id, cursor=cursor, limit=limit)
        print("The response of SinksApi->list_sink_enablements:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling SinksApi->list_sink_enablements: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Project identifier (UUIDv7). Bound on &#x60;/v1/projects/{id}&#x60; for the tenancy CRUD surface, on &#x60;/v1/projects/{id}/credentials&#x60; for the operator-facing OpenBao Credential Broker inventory list, on &#x60;/v1/projects/{id}/credential-assignments&#x60; and &#x60;/v1/projects/{id}/cloud-assignments&#x60; for the assignment request/list surfaces, on &#x60;/v1/projects/{id}/blueprints&#x60; for the project-scoped Blueprint offer list, and on &#x60;/v1/projects/{id}/sink-enablements&#x60; and &#x60;/v1/projects/{id}/telemetry-routes&#x60; for the sink-enablement and Telemetry Route surfaces.  | 
 **cursor** | **str**| Opaque continuation token returned by a previous call&#39;s &#x60;next_cursor&#x60;. The encoding is HMAC-signed by the server so a tampered cursor surfaces as &#x60;400&#x60;.  | [optional] 
 **limit** | **int**| Maximum number of items to return in a single page. A value outside [1, 200] is rejected with a &#x60;400&#x60; Problem rather than silently clamped.  | [optional] [default to 50]

### Return type

[**SinkEnablementPage**](SinkEnablementPage.md)

### Authorization

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Page of sink enablements. |  -  |
**400** | Invalid query parameters — a tampered or malformed cursor (&#x60;code: invalid_cursor&#x60;), an out-of-range &#x60;limit&#x60; (&#x60;code: invalid_limit&#x60;), or a malformed &#x60;{id}&#x60; (&#x60;code: invalid_project_id&#x60;).  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the &#x60;read&#x60; ReBAC permission on the addressed Project (body is a &#x60;PermissionDenied&#x60; problem) OR the pagination cursor was minted by a different caller and the per-(caller, pepper) HMAC binding rejected the replay (body is a &#x60;Problem&#x60; with &#x60;code: cursor_binding_mismatch&#x60;).  |  -  |
**404** | The addressed Project does not exist. The Problem body&#39;s &#x60;code&#x60; field is &#x60;project_not_found&#x60;.  |  -  |
**500** | Internal server error. |  -  |
**501** | The sink-enablement surface is not yet provisioned in this build, so log scrapers can alert on the deferred-wiring state. The Problem body&#39;s &#x60;code&#x60; field is &#x60;sink_enablements_not_provisioned&#x60;.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **list_telemetry_routes**
> TelemetryRouteList list_telemetry_routes(id)

List the Telemetry Routes a Project holds.

Returns the Telemetry Routes of the Project addressed by `{id}`,
in creation order. The read is gated by the Project's `read` ReBAC
permission, checked before the persistence read; every route
belongs to the one path Project, so that check authorises the
whole list.

A Project holds a bounded number of routes, so the listing is not
paginated.


### Example

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

```python
import plexsphere
from plexsphere.models.telemetry_route_list import TelemetryRouteList
from plexsphere.rest import ApiException
from pprint import pprint

# Defining the host is optional and defaults to http://localhost
# See configuration.py for a list of all supported configuration parameters.
configuration = plexsphere.Configuration(
    host = "http://localhost"
)

# The client must configure the authentication and authorization parameters
# in accordance with the API server security policy.
# Examples for each auth method are provided below, use the example that
# satisfies your auth use case.

# Configure Bearer authorization (JWT): operatorBearer
configuration = plexsphere.Configuration(
    access_token = os.environ["BEARER_TOKEN"]
)

# Configure API key authorization: sessionCookie
configuration.api_key['sessionCookie'] = os.environ["API_KEY"]

# Uncomment below to setup prefix (e.g. Bearer) for API key, if needed
# configuration.api_key_prefix['sessionCookie'] = 'Bearer'

# Enter a context with an instance of the API client
with plexsphere.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = plexsphere.SinksApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Project identifier (UUIDv7). Bound on `/v1/projects/{id}` for the tenancy CRUD surface, on `/v1/projects/{id}/credentials` for the operator-facing OpenBao Credential Broker inventory list, on `/v1/projects/{id}/credential-assignments` and `/v1/projects/{id}/cloud-assignments` for the assignment request/list surfaces, on `/v1/projects/{id}/blueprints` for the project-scoped Blueprint offer list, and on `/v1/projects/{id}/sink-enablements` and `/v1/projects/{id}/telemetry-routes` for the sink-enablement and Telemetry Route surfaces. 

    try:
        # List the Telemetry Routes a Project holds.
        api_response = api_instance.list_telemetry_routes(id)
        print("The response of SinksApi->list_telemetry_routes:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling SinksApi->list_telemetry_routes: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Project identifier (UUIDv7). Bound on &#x60;/v1/projects/{id}&#x60; for the tenancy CRUD surface, on &#x60;/v1/projects/{id}/credentials&#x60; for the operator-facing OpenBao Credential Broker inventory list, on &#x60;/v1/projects/{id}/credential-assignments&#x60; and &#x60;/v1/projects/{id}/cloud-assignments&#x60; for the assignment request/list surfaces, on &#x60;/v1/projects/{id}/blueprints&#x60; for the project-scoped Blueprint offer list, and on &#x60;/v1/projects/{id}/sink-enablements&#x60; and &#x60;/v1/projects/{id}/telemetry-routes&#x60; for the sink-enablement and Telemetry Route surfaces.  | 

### Return type

[**TelemetryRouteList**](TelemetryRouteList.md)

### Authorization

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | The Telemetry Routes the Project holds. |  -  |
**400** | A malformed &#x60;{id}&#x60;. The Problem body&#39;s &#x60;code&#x60; field is &#x60;invalid_project_id&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the &#x60;read&#x60; ReBAC permission on the addressed Project.  |  -  |
**404** | The addressed Project does not exist. The Problem body&#39;s &#x60;code&#x60; field is &#x60;project_not_found&#x60;.  |  -  |
**500** | Internal server error. |  -  |
**501** | The Telemetry Route surface is not yet provisioned in this build, so log scrapers can alert on the deferred-wiring state. The Problem body&#39;s &#x60;code&#x60; field is &#x60;telemetry_routes_not_provisioned&#x60;.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **request_sink_enablement**
> SinkEnablement request_sink_enablement(id, sink_enablement_request_body)

Request a sink enablement for a Project.

Opens a sink-enablement request that asks for the sink named in
the body to be made usable in the Project addressed by `{id}`.
The application service checks the Project's `deploy` permission
before the write, records the enablement in the `requested`
state, and appends a `SinkEnablementRequested` outbox event in
the same transaction.

A request grants nothing. It is decided on the approvals queue,
where it surfaces as a row of kind `sink_enablement`:
`POST /v1/approvals/{id}/approve` and
`POST /v1/approvals/{id}/reject` settle it, and the four-eyes
rule refuses a decision by the requester.

Only a tenant sink is enableable — a built-in sink is reachable
without a grant, so naming one is refused with
`422 sink_not_enableable`. The sink must belong to the Project's
own Domain, otherwise `422 sink_domain_mismatch`. A second live
(requested or approved) enablement for the same (Project, sink)
pair is refused with `409 duplicate_live_sink_enablement`.


### Example

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

```python
import plexsphere
from plexsphere.models.sink_enablement import SinkEnablement
from plexsphere.models.sink_enablement_request_body import SinkEnablementRequestBody
from plexsphere.rest import ApiException
from pprint import pprint

# Defining the host is optional and defaults to http://localhost
# See configuration.py for a list of all supported configuration parameters.
configuration = plexsphere.Configuration(
    host = "http://localhost"
)

# The client must configure the authentication and authorization parameters
# in accordance with the API server security policy.
# Examples for each auth method are provided below, use the example that
# satisfies your auth use case.

# Configure Bearer authorization (JWT): operatorBearer
configuration = plexsphere.Configuration(
    access_token = os.environ["BEARER_TOKEN"]
)

# Configure API key authorization: sessionCookie
configuration.api_key['sessionCookie'] = os.environ["API_KEY"]

# Uncomment below to setup prefix (e.g. Bearer) for API key, if needed
# configuration.api_key_prefix['sessionCookie'] = 'Bearer'

# Enter a context with an instance of the API client
with plexsphere.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = plexsphere.SinksApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Project identifier (UUIDv7). Bound on `/v1/projects/{id}` for the tenancy CRUD surface, on `/v1/projects/{id}/credentials` for the operator-facing OpenBao Credential Broker inventory list, on `/v1/projects/{id}/credential-assignments` and `/v1/projects/{id}/cloud-assignments` for the assignment request/list surfaces, on `/v1/projects/{id}/blueprints` for the project-scoped Blueprint offer list, and on `/v1/projects/{id}/sink-enablements` and `/v1/projects/{id}/telemetry-routes` for the sink-enablement and Telemetry Route surfaces. 
    sink_enablement_request_body = {"sink_id":"0190a8b8-a0c0-7a0a-8a0a-a0a0a0a0a0f1"} # SinkEnablementRequestBody | 

    try:
        # Request a sink enablement for a Project.
        api_response = api_instance.request_sink_enablement(id, sink_enablement_request_body)
        print("The response of SinksApi->request_sink_enablement:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling SinksApi->request_sink_enablement: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Project identifier (UUIDv7). Bound on &#x60;/v1/projects/{id}&#x60; for the tenancy CRUD surface, on &#x60;/v1/projects/{id}/credentials&#x60; for the operator-facing OpenBao Credential Broker inventory list, on &#x60;/v1/projects/{id}/credential-assignments&#x60; and &#x60;/v1/projects/{id}/cloud-assignments&#x60; for the assignment request/list surfaces, on &#x60;/v1/projects/{id}/blueprints&#x60; for the project-scoped Blueprint offer list, and on &#x60;/v1/projects/{id}/sink-enablements&#x60; and &#x60;/v1/projects/{id}/telemetry-routes&#x60; for the sink-enablement and Telemetry Route surfaces.  | 
 **sink_enablement_request_body** | [**SinkEnablementRequestBody**](SinkEnablementRequestBody.md)|  | 

### Return type

[**SinkEnablement**](SinkEnablement.md)

### Authorization

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**201** | The enablement was opened in the &#x60;requested&#x60; state. No &#x60;Location&#x60; header is sent — the aggregate has no by-id read surface; poll it through the Project&#39;s enablement list.  |  -  |
**400** | A malformed &#x60;{id}&#x60; or an unreadable body. The Problem body&#39;s &#x60;code&#x60; field is &#x60;invalid_project_id&#x60; or &#x60;invalid_body&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the &#x60;deploy&#x60; ReBAC permission on the addressed Project.  |  -  |
**404** | The addressed Project or the named sink does not exist. The Problem body&#39;s &#x60;code&#x60; field is &#x60;project_not_found&#x60; or &#x60;sink_not_found&#x60;.  |  -  |
**409** | A live enablement already exists for the same (Project, sink) pair. The Problem body&#39;s &#x60;code&#x60; field is &#x60;duplicate_live_sink_enablement&#x60;.  |  -  |
**422** | Semantic rejection. The Problem body&#39;s &#x60;code&#x60; field is &#x60;sink_not_enableable&#x60; when the named sink is built in, or &#x60;sink_domain_mismatch&#x60; when it belongs to a Domain other than the Project&#39;s.  |  -  |
**500** | Internal server error. |  -  |
**501** | The sink-enablement surface is not yet provisioned in this build, so log scrapers can alert on the deferred-wiring state. The Problem body&#39;s &#x60;code&#x60; field is &#x60;sink_enablements_not_provisioned&#x60;.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **revoke_sink_enablement**
> SinkEnablement revoke_sink_enablement(id, sink_enablement_revoke_body)

Revoke a sink enablement.

Withdraws the approved enablement addressed by `{id}`. The
application service runs a two-leg gate — the sink's `assign`
permission, and on its denial the consuming Project's `deploy`
permission — then moves the enablement to `revoked`, deletes the
`uses` edge, and appends a `SinkEnablementRevoked` outbox event.
The `reason` from the body is recorded on the decision as an
operator-supplied audit string.

Revocation is legal from `approved` only; any other source state
returns `409 illegal_transition`.

A `502 revocation_sync_pending` means the row moved to `revoked`
and its event is durable, but the edge delete did not complete —
the Project still holds the grant and keeps delivering. The
direction is fail-open, which is why it is reported as a failure
rather than as a completed revocation. Retrying is refused
(`revoked` is terminal); the removal is left to the committed
event replaying through the authz-sync arm.


### Example

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

```python
import plexsphere
from plexsphere.models.sink_enablement import SinkEnablement
from plexsphere.models.sink_enablement_revoke_body import SinkEnablementRevokeBody
from plexsphere.rest import ApiException
from pprint import pprint

# Defining the host is optional and defaults to http://localhost
# See configuration.py for a list of all supported configuration parameters.
configuration = plexsphere.Configuration(
    host = "http://localhost"
)

# The client must configure the authentication and authorization parameters
# in accordance with the API server security policy.
# Examples for each auth method are provided below, use the example that
# satisfies your auth use case.

# Configure Bearer authorization (JWT): operatorBearer
configuration = plexsphere.Configuration(
    access_token = os.environ["BEARER_TOKEN"]
)

# Configure API key authorization: sessionCookie
configuration.api_key['sessionCookie'] = os.environ["API_KEY"]

# Uncomment below to setup prefix (e.g. Bearer) for API key, if needed
# configuration.api_key_prefix['sessionCookie'] = 'Bearer'

# Enter a context with an instance of the API client
with plexsphere.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = plexsphere.SinksApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Sink enablement identifier (UUIDv7). Bound on `/v1/sink-enablements/{id}/revoke` for the grant withdrawal. 
    sink_enablement_revoke_body = {"reason":"Collector decommissioned by the platform on-call"} # SinkEnablementRevokeBody | 

    try:
        # Revoke a sink enablement.
        api_response = api_instance.revoke_sink_enablement(id, sink_enablement_revoke_body)
        print("The response of SinksApi->revoke_sink_enablement:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling SinksApi->revoke_sink_enablement: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Sink enablement identifier (UUIDv7). Bound on &#x60;/v1/sink-enablements/{id}/revoke&#x60; for the grant withdrawal.  | 
 **sink_enablement_revoke_body** | [**SinkEnablementRevokeBody**](SinkEnablementRevokeBody.md)|  | 

### Return type

[**SinkEnablement**](SinkEnablement.md)

### Authorization

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | The enablement was revoked. The body is the projection with &#x60;state: revoked&#x60;.  |  -  |
**400** | A malformed &#x60;{id}&#x60; or an unreadable body. The Problem body&#39;s &#x60;code&#x60; field is &#x60;invalid_sink_enablement_id&#x60;, &#x60;invalid_body&#x60;, or &#x60;invalid_decision_reason&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller holds neither &#x60;assign&#x60; on the sink nor &#x60;deploy&#x60; on the consuming Project.  |  -  |
**404** | No enablement with the id exists. The Problem body&#39;s &#x60;code&#x60; field is &#x60;sink_enablement_not_found&#x60;.  |  -  |
**409** | The enablement is not in a state revocation is legal from. The Problem body&#39;s &#x60;code&#x60; field is &#x60;illegal_transition&#x60;.  |  -  |
**500** | Internal server error. |  -  |
**501** | The sink-enablement surface is not yet provisioned in this build, so log scrapers can alert on the deferred-wiring state. The Problem body&#39;s &#x60;code&#x60; field is &#x60;sink_enablements_not_provisioned&#x60;.  |  -  |
**502** | The revocation committed but the grant is still live in the authorization graph. The Problem body&#39;s &#x60;code&#x60; field is &#x60;revocation_sync_pending&#x60;.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **update_sink**
> Sink update_sink(id, sink_update_request)

Replace a tenant sink's mutable state.

Replaces the mutable state of the tenant sink addressed by `{id}`
with the post-image the body states. Every mutable field is
restated on each call: a field absent from the body is cleared,
not left untouched.

`id`, `domain_id` and `built_in` are immutable and are therefore
not part of the body. An update that would move the sink to
another Domain or flip its built-in discriminator is refused with
`409 sink_conflict`, as is an update addressing a built-in sink.

The write is guarded by a compare-and-swap precondition: the body
states the `updated_at` the client read the sink at, and the
update is applied only if the stored sink still carries it.
Because the body replaces the whole sink, two clients editing
from the same listing would otherwise resolve last-writer-wins,
and the loser's post-image would silently restore the security
posture the winner had just changed — re-enabling
`tls.insecure_skip_verify`, restoring a detached `credential`, or
reverting a rotated `endpoint`. A stale token is refused with
`409 sink_cas_conflict` and nothing is written.

The handler checks the `manage` ReBAC permission on the addressed
sink before any write. `credential` restates the KV mount and
version only; the path stays derived from the owning Domain and
the sink id.


### Example

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

```python
import plexsphere
from plexsphere.models.sink import Sink
from plexsphere.models.sink_update_request import SinkUpdateRequest
from plexsphere.rest import ApiException
from pprint import pprint

# Defining the host is optional and defaults to http://localhost
# See configuration.py for a list of all supported configuration parameters.
configuration = plexsphere.Configuration(
    host = "http://localhost"
)

# The client must configure the authentication and authorization parameters
# in accordance with the API server security policy.
# Examples for each auth method are provided below, use the example that
# satisfies your auth use case.

# Configure Bearer authorization (JWT): operatorBearer
configuration = plexsphere.Configuration(
    access_token = os.environ["BEARER_TOKEN"]
)

# Configure API key authorization: sessionCookie
configuration.api_key['sessionCookie'] = os.environ["API_KEY"]

# Uncomment below to setup prefix (e.g. Bearer) for API key, if needed
# configuration.api_key_prefix['sessionCookie'] = 'Bearer'

# Enter a context with an instance of the API client
with plexsphere.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = plexsphere.SinksApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Sink identifier (UUIDv7). Bound on `/v1/sinks/{id}` for the single-sink read, replace, and delete, and on `/v1/sinks/{id}/sink-enablements` for the owner-push grant. 
    sink_update_request = {"expected_updated_at":"2026-08-19T09:41:12.183742Z","slug":"central-otlp","display_name":"Central OTLP collector (EU)","sink_type":"otlp","endpoint":"https://otlp.eu.example.com:4317","settings":{"dataset":"production"}} # SinkUpdateRequest | 

    try:
        # Replace a tenant sink's mutable state.
        api_response = api_instance.update_sink(id, sink_update_request)
        print("The response of SinksApi->update_sink:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling SinksApi->update_sink: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Sink identifier (UUIDv7). Bound on &#x60;/v1/sinks/{id}&#x60; for the single-sink read, replace, and delete, and on &#x60;/v1/sinks/{id}/sink-enablements&#x60; for the owner-push grant.  | 
 **sink_update_request** | [**SinkUpdateRequest**](SinkUpdateRequest.md)|  | 

### Return type

[**Sink**](Sink.md)

### Authorization

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | The updated sink. |  -  |
**400** | A malformed &#x60;{id}&#x60; or an unreadable body. The Problem body&#39;s &#x60;code&#x60; field is &#x60;invalid_sink_id&#x60; or &#x60;invalid_body&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the &#x60;manage&#x60; ReBAC permission on the addressed sink.  |  -  |
**404** | No sink with the id exists. The Problem body&#39;s &#x60;code&#x60; field is &#x60;sink_not_found&#x60;.  |  -  |
**409** | The write cannot be applied against the stored sink. The Problem body&#39;s &#x60;code&#x60; field is &#x60;sink_slug_taken&#x60; when the new slug is already held in the Domain, &#x60;sink_conflict&#x60; when the post-image would move the sink to another Domain, flip its built-in discriminator, or addresses a built-in sink, or &#x60;sink_cas_conflict&#x60; when the sink changed since the &#x60;expected_updated_at&#x60; the body states.  |  -  |
**422** | The aggregate refused the post-image. The Problem body&#39;s &#x60;code&#x60; field is &#x60;sink_invalid&#x60;.  |  -  |
**500** | Internal server error. |  -  |
**501** | The sink surface is not yet provisioned in this build, so log scrapers can alert on the deferred-wiring state. The Problem body&#39;s &#x60;code&#x60; field is &#x60;sinks_not_provisioned&#x60;.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **update_telemetry_route**
> TelemetryRoute update_telemetry_route(id, telemetry_route_request)

Replace a Telemetry Route's state.

Replaces the Telemetry Route addressed by `{id}` with the
post-image the body states. Every mutable field is restated on
each call: a field absent from the body is cleared, not left
untouched.

The stored `project_id` is immutable and is therefore not part of
the body — the update is applied against the Project the route
already belongs to. A post-image that names a different Project is
refused with `422 route_project_mismatch`, the service-level guard
behind the transport's use of the stored id.

The handler resolves the route to learn its Project, then checks
that Project's `deploy` ReBAC permission before the write. Every
target and predicate rule the create path enforces applies
unchanged.


### Example

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

```python
import plexsphere
from plexsphere.models.telemetry_route import TelemetryRoute
from plexsphere.models.telemetry_route_request import TelemetryRouteRequest
from plexsphere.rest import ApiException
from pprint import pprint

# Defining the host is optional and defaults to http://localhost
# See configuration.py for a list of all supported configuration parameters.
configuration = plexsphere.Configuration(
    host = "http://localhost"
)

# The client must configure the authentication and authorization parameters
# in accordance with the API server security policy.
# Examples for each auth method are provided below, use the example that
# satisfies your auth use case.

# Configure Bearer authorization (JWT): operatorBearer
configuration = plexsphere.Configuration(
    access_token = os.environ["BEARER_TOKEN"]
)

# Configure API key authorization: sessionCookie
configuration.api_key['sessionCookie'] = os.environ["API_KEY"]

# Uncomment below to setup prefix (e.g. Bearer) for API key, if needed
# configuration.api_key_prefix['sessionCookie'] = 'Bearer'

# Enter a context with an instance of the API client
with plexsphere.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = plexsphere.SinksApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Telemetry Route identifier (UUIDv7). Bound on `/v1/telemetry-routes/{id}` for the single-route read, replace, and delete. 
    telemetry_route_request = {"signal":"logs","severity_floor":"err","sink_ids":["0190a8b8-a0c0-7a0a-8a0a-a0a0a0a0a0f1","0190a8b8-a0c0-7a0a-8a0a-a0a0a0a0a0f2"]} # TelemetryRouteRequest | 

    try:
        # Replace a Telemetry Route's state.
        api_response = api_instance.update_telemetry_route(id, telemetry_route_request)
        print("The response of SinksApi->update_telemetry_route:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling SinksApi->update_telemetry_route: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Telemetry Route identifier (UUIDv7). Bound on &#x60;/v1/telemetry-routes/{id}&#x60; for the single-route read, replace, and delete.  | 
 **telemetry_route_request** | [**TelemetryRouteRequest**](TelemetryRouteRequest.md)|  | 

### Return type

[**TelemetryRoute**](TelemetryRoute.md)

### Authorization

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | The updated Telemetry Route. |  -  |
**400** | A malformed &#x60;{id}&#x60; or an unreadable body. The Problem body&#39;s &#x60;code&#x60; field is &#x60;invalid_telemetry_route_id&#x60; or &#x60;invalid_body&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the &#x60;deploy&#x60; ReBAC permission on the Project the route belongs to.  |  -  |
**404** | No route with the id exists. The Problem body&#39;s &#x60;code&#x60; field is &#x60;telemetry_route_not_found&#x60;.  |  -  |
**422** | Semantic rejection. The Problem body&#39;s &#x60;code&#x60; field is &#x60;telemetry_route_invalid&#x60;, &#x60;route_sink_not_found&#x60;, &#x60;route_sink_not_usable&#x60;, &#x60;route_signal_not_accepted&#x60;, or &#x60;route_project_mismatch&#x60;.  |  -  |
**500** | Internal server error. |  -  |
**501** | The Telemetry Route surface is not yet provisioned in this build, so log scrapers can alert on the deferred-wiring state. The Problem body&#39;s &#x60;code&#x60; field is &#x60;telemetry_routes_not_provisioned&#x60;.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

