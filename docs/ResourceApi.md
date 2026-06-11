# plexsphere.ResourceApi

All URIs are relative to *http://localhost*

Method | HTTP request | Description
------------- | ------------- | -------------
[**create_resource**](ResourceApi.md#create_resource) | **POST** /v1/projects/{project_id}/resources | Create a Resource (adopted or broker-provisioned).
[**delete_resource**](ResourceApi.md#delete_resource) | **DELETE** /v1/resources/{id} | Delete a Resource (graceful for provisioned).
[**get_resource**](ResourceApi.md#get_resource) | **GET** /v1/resources/{id} | Fetch a Resource by identifier.
[**list_project_resources**](ResourceApi.md#list_project_resources) | **GET** /v1/projects/{project_id}/resources | List Resources inside a Project.


# **create_resource**
> ResourceResponse create_resource(project_id, resource_create_request)

Create a Resource (adopted or broker-provisioned).

Creates a Tenancy Resource inside the addressed Project. The
`origin` field discriminates the two creation flows:

  * `origin=adopted` — the Resource entered the platform
    through an out-of-band channel (bare-metal systemd unit,
    Kubernetes ExternalSecrets pattern). The handler persists
    the Resource with `Origin=Adopted` and returns `201` with
    the hydrated `ResourceResponse`.
  * `origin=provisioned` — the Resource's substrate is owned
    by the Provisioning Broker. The handler persists the
    Resource with `Origin=Provisioned`, calls
    `broker.Service.Provision` to mint a Pending
    `ProvisionedResource`, and returns `202` with a pollable
    `Location` header pointing at `/v1/resources/{id}`. The
    provisioned flow additionally requires the
    `cloud_credential#uses` ReBAC relation on the named
    Cloud Credential.

Every call runs the Project `deploy` ReBAC check BEFORE any
persistence write. A body missing `origin` or carrying a value
outside `{adopted, provisioned}` is rejected with
`400 invalid_resource_origin`.


### Example


```python
import plexsphere
from plexsphere.models.resource_create_request import ResourceCreateRequest
from plexsphere.models.resource_response import ResourceResponse
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
    api_instance = plexsphere.ResourceApi(api_client)
    project_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Project (UUIDv7). Bound on `/v1/projects/{project_id}/resources` for the Resource collection — create and list. Reused by the Bridge Orchestrator surface under `/v1/projects/{project_id}/resources/{resource_id}/bridge/...`. 
    resource_create_request = {"origin":"adopted","kind":"kubernetes-node","external_ref":"node-ref-01"} # ResourceCreateRequest | 

    try:
        # Create a Resource (adopted or broker-provisioned).
        api_response = api_instance.create_resource(project_id, resource_create_request)
        print("The response of ResourceApi->create_resource:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling ResourceApi->create_resource: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **project_id** | **UUID**| Owning Project (UUIDv7). Bound on &#x60;/v1/projects/{project_id}/resources&#x60; for the Resource collection — create and list. Reused by the Bridge Orchestrator surface under &#x60;/v1/projects/{project_id}/resources/{resource_id}/bridge/...&#x60;.  | 
 **resource_create_request** | [**ResourceCreateRequest**](ResourceCreateRequest.md)|  | 

### Return type

[**ResourceResponse**](ResourceResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**201** | Adopted Resource created. The response body is the hydrated &#x60;ResourceResponse&#x60;; the &#x60;provisioning&#x60; field is absent because adopted Resources have no broker-owned ProvisionedResource.  |  -  |
**202** | Provisioned Resource accepted. The broker has minted a Pending &#x60;ProvisionedResource&#x60; and reconcile will drive it toward &#x60;Ready&#x60;. The response body carries the hydrated &#x60;ResourceResponse&#x60; with &#x60;provisioning.phase &#x3D; Pending&#x60;; the &#x60;Location&#x60; header carries the canonical URL of the new Resource.  |  * Location - Absolute path of the issued Session for the single-Session read.  <br>  |
**400** | Body rejected. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;invalid_resource_origin&#x60;, &#x60;invalid_project_id&#x60;, &#x60;invalid_body&#x60; }.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to create a Resource in this Project (missing Project &#x60;deploy&#x60;) or to use the named Cloud Credential (missing &#x60;cloud_credential#uses&#x60; for the provisioned flow). Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Parent Project not found. Body is a &#x60;Problem&#x60; with &#x60;code: project_not_found&#x60;.  |  -  |
**413** | Request body exceeded the 8 KiB tenancy ceiling.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **delete_resource**
> delete_resource(id)

Delete a Resource (graceful for provisioned).

Deletes the Resource identified by `{id}`. The handler
branches on the Resource's `origin`:

  * `Origin=Provisioned` — triggers `broker.Service.Deprovision`,
    which advances the broker aggregate into the teardown
    phase and emits the `ProvisionedResourceDeleting` outbox
    event. Returns `202`; the caller polls
    `GET /v1/resources/{id}` to observe phase progression.
  * `Origin=Adopted` — removes the tenancy Resource row
    directly. Returns `204`.

Every call runs the Project `deploy` ReBAC check BEFORE any
persistence write. A missing Resource surfaces as
`404 resource_not_found`.


### Example


```python
import plexsphere
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
    api_instance = plexsphere.ResourceApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Resource identifier (UUIDv7). Bound on `/v1/resources/{id}` for the Tenancy Resource read + delete surface. 

    try:
        # Delete a Resource (graceful for provisioned).
        api_instance.delete_resource(id)
    except Exception as e:
        print("Exception when calling ResourceApi->delete_resource: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Resource identifier (UUIDv7). Bound on &#x60;/v1/resources/{id}&#x60; for the Tenancy Resource read + delete surface.  | 

### Return type

void (empty response body)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**202** | Provisioned Resource deletion accepted. The broker has stamped the teardown phase and the reconcile loop will drive the aggregate toward &#x60;Deleted&#x60;. Poll &#x60;GET /v1/resources/{id}&#x60; for phase progression.  |  -  |
**204** | Adopted Resource deleted. |  -  |
**400** | Malformed Resource id. Body is a &#x60;Problem&#x60; with &#x60;code: invalid_resource_id&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to delete the addressed Resource. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Resource not found. Body is a &#x60;Problem&#x60; with &#x60;code: resource_not_found&#x60;.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **get_resource**
> ResourceResponse get_resource(id)

Fetch a Resource by identifier.

Returns the Resource identified by `{id}`. For
`Origin=Provisioned` Resources the response carries the
`provisioning` sub-object reporting the broker phase the
reconcile loop most recently observed. For `Origin=Adopted`
Resources the `provisioning` field is absent.

The handler runs the `observe` ReBAC check on the parent
Project BEFORE the persistence read; an unauthorised caller
therefore receives `403` without the existence side-channel a
"load-then-check" flow would leak. A missing Resource surfaces
as `404 resource_not_found`.


### Example


```python
import plexsphere
from plexsphere.models.resource_response import ResourceResponse
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
    api_instance = plexsphere.ResourceApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Resource identifier (UUIDv7). Bound on `/v1/resources/{id}` for the Tenancy Resource read + delete surface. 

    try:
        # Fetch a Resource by identifier.
        api_response = api_instance.get_resource(id)
        print("The response of ResourceApi->get_resource:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling ResourceApi->get_resource: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Resource identifier (UUIDv7). Bound on &#x60;/v1/resources/{id}&#x60; for the Tenancy Resource read + delete surface.  | 

### Return type

[**ResourceResponse**](ResourceResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Resource found. |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to read the addressed Resource. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Resource not found. Body is a &#x60;Problem&#x60; with &#x60;code: resource_not_found&#x60;.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **list_project_resources**
> ResourceList list_project_resources(project_id, cursor=cursor, limit=limit)

List Resources inside a Project.

Returns a creation-ordered page of Resources owned by the
Project identified by `{project_id}`. The handler runs a
top-level `observe` ReBAC check on the parent Project BEFORE
the persistence read, then layers a per-row `observe` filter
on top so the response items are the subset of the
persistence-level page the caller is authorised to see.

The pagination cursor is HMAC-signed and bound to the
per-(caller, pepper) pseudonym, so a cursor minted by one
principal cannot be replayed by another — the cross-caller
replay surfaces as `403 cursor_binding_mismatch`. A tampered
envelope or unknown version byte stays on `400 invalid_cursor`.


### Example


```python
import plexsphere
from plexsphere.models.resource_list import ResourceList
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
    api_instance = plexsphere.ResourceApi(api_client)
    project_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Project (UUIDv7). Bound on `/v1/projects/{project_id}/resources` for the Resource collection — create and list. Reused by the Bridge Orchestrator surface under `/v1/projects/{project_id}/resources/{resource_id}/bridge/...`. 
    cursor = 'cursor_example' # str | Opaque continuation token returned by a previous call's `next_cursor`. The encoding is HMAC-signed by the server so a tampered cursor surfaces as `400`.  (optional)
    limit = 50 # int | Maximum number of items to return in a single page. The handler clamps the value to [1, 200] before forwarding it to the read service.  (optional) (default to 50)

    try:
        # List Resources inside a Project.
        api_response = api_instance.list_project_resources(project_id, cursor=cursor, limit=limit)
        print("The response of ResourceApi->list_project_resources:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling ResourceApi->list_project_resources: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **project_id** | **UUID**| Owning Project (UUIDv7). Bound on &#x60;/v1/projects/{project_id}/resources&#x60; for the Resource collection — create and list. Reused by the Bridge Orchestrator surface under &#x60;/v1/projects/{project_id}/resources/{resource_id}/bridge/...&#x60;.  | 
 **cursor** | **str**| Opaque continuation token returned by a previous call&#39;s &#x60;next_cursor&#x60;. The encoding is HMAC-signed by the server so a tampered cursor surfaces as &#x60;400&#x60;.  | [optional] 
 **limit** | **int**| Maximum number of items to return in a single page. The handler clamps the value to [1, 200] before forwarding it to the read service.  | [optional] [default to 50]

### Return type

[**ResourceList**](ResourceList.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Page of Resources. |  -  |
**400** | Invalid query parameters — typically a malformed Project id, a tampered or malformed cursor, or an out-of-range &#x60;limit&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to observe the parent Project (body is a &#x60;PermissionDenied&#x60; problem) OR the pagination cursor was minted by a different caller and the per-(caller, pepper) HMAC binding rejected the replay (body is a &#x60;Problem&#x60; with &#x60;code &#x3D; cursor_binding_mismatch&#x60;).  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

