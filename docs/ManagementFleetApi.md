# plexsphere.ManagementFleetApi

All URIs are relative to *http://localhost*

Method | HTTP request | Description
------------- | ------------- | -------------
[**get_management_cluster**](ManagementFleetApi.md#get_management_cluster) | **GET** /v1/management-clusters/{id} | Fetch one management cluster.
[**get_project_management_cluster_assignment**](ManagementFleetApi.md#get_project_management_cluster_assignment) | **GET** /v1/projects/{project_id}/management-cluster-assignment | Look up a Project&#39;s management-cluster assignment.
[**list_management_cluster_assignments**](ManagementFleetApi.md#list_management_cluster_assignments) | **GET** /v1/management-clusters/{id}/assignments | List the Project assignments placed on a cluster.
[**list_management_clusters**](ManagementFleetApi.md#list_management_clusters) | **GET** /v1/management-clusters | List the registered management clusters.
[**register_management_cluster**](ManagementFleetApi.md#register_management_cluster) | **POST** /v1/management-clusters | Register a management cluster into the fleet.
[**terminate_project_management_cluster_assignment**](ManagementFleetApi.md#terminate_project_management_cluster_assignment) | **POST** /v1/projects/{project_id}/management-cluster-assignment/terminate | Request teardown of a Project&#39;s namespace.


# **get_management_cluster**
> ManagementClusterResponse get_management_cluster(id)

Fetch one management cluster.

Returns the metadata for the management cluster identified by
`{id}`. The handler runs an `observe` ReBAC check on
`managementcluster:{id}` (which derives from the fleet
singleton, so a fleet observer transitively observes every
cluster). A missing row surfaces as
`404 management_cluster_not_found`.


### Example


```python
import plexsphere
from plexsphere.models.management_cluster_response import ManagementClusterResponse
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
    api_instance = plexsphere.ManagementFleetApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Management cluster identifier (UUIDv7). Bound on `/v1/management-clusters/{id}` and `/v1/management-clusters/{id}/assignments` for the operator-facing Management Fleet surface. 

    try:
        # Fetch one management cluster.
        api_response = api_instance.get_management_cluster(id)
        print("The response of ManagementFleetApi->get_management_cluster:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling ManagementFleetApi->get_management_cluster: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Management cluster identifier (UUIDv7). Bound on &#x60;/v1/management-clusters/{id}&#x60; and &#x60;/v1/management-clusters/{id}/assignments&#x60; for the operator-facing Management Fleet surface.  | 

### Return type

[**ManagementClusterResponse**](ManagementClusterResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Management cluster found. |  -  |
**400** | Malformed cluster id. Body is a &#x60;Problem&#x60; with &#x60;code: invalid_management_cluster_id&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to observe the cluster. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Management cluster not found. Body is a &#x60;Problem&#x60; with &#x60;code: management_cluster_not_found&#x60;.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **get_project_management_cluster_assignment**
> ProjectClusterAssignmentResponse get_project_management_cluster_assignment(project_id)

Look up a Project's management-cluster assignment.

Returns the management-cluster assignment for the Project
identified by `{project_id}`, including the derived namespace
name and its lifecycle phase. The handler runs an `observe`
ReBAC check on the `managementfleet:fleet` singleton — the
caller need not know the resolved cluster id up front and the
fleet gate avoids a pre-authz read existence oracle. A Project
that has never been placed surfaces as
`404 management_assignment_not_found`.


### Example


```python
import plexsphere
from plexsphere.models.project_cluster_assignment_response import ProjectClusterAssignmentResponse
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
    api_instance = plexsphere.ManagementFleetApi(api_client)
    project_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Project (UUIDv7). Bound on `/v1/projects/{project_id}/management-cluster-assignment` and its `/terminate` sub-resource — the Management Fleet assignment is keyed by the Project it places. 

    try:
        # Look up a Project's management-cluster assignment.
        api_response = api_instance.get_project_management_cluster_assignment(project_id)
        print("The response of ManagementFleetApi->get_project_management_cluster_assignment:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling ManagementFleetApi->get_project_management_cluster_assignment: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **project_id** | **UUID**| Owning Project (UUIDv7). Bound on &#x60;/v1/projects/{project_id}/management-cluster-assignment&#x60; and its &#x60;/terminate&#x60; sub-resource — the Management Fleet assignment is keyed by the Project it places.  | 

### Return type

[**ProjectClusterAssignmentResponse**](ProjectClusterAssignmentResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | The Project&#39;s assignment. |  -  |
**400** | Malformed Project id. Body is a &#x60;Problem&#x60; with &#x60;code: invalid_project_id&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to observe the management fleet. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | The Project has no management-cluster assignment. Body is a &#x60;Problem&#x60; with &#x60;code: management_assignment_not_found&#x60;.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **list_management_cluster_assignments**
> ProjectClusterAssignmentList list_management_cluster_assignments(id)

List the Project assignments placed on a cluster.

Returns the project-id-ordered set of Project ↔ cluster
assignments placed on the management cluster identified by
`{id}`, including each namespace's lifecycle phase. The handler
runs an `observe` ReBAC check on `managementcluster:{id}`
before the read. An unknown cluster id yields an empty list —
the assignment table carries no cluster existence oracle and
the gate already authorised the caller against the cluster
object.


### Example


```python
import plexsphere
from plexsphere.models.project_cluster_assignment_list import ProjectClusterAssignmentList
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
    api_instance = plexsphere.ManagementFleetApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Management cluster identifier (UUIDv7). Bound on `/v1/management-clusters/{id}` and `/v1/management-clusters/{id}/assignments` for the operator-facing Management Fleet surface. 

    try:
        # List the Project assignments placed on a cluster.
        api_response = api_instance.list_management_cluster_assignments(id)
        print("The response of ManagementFleetApi->list_management_cluster_assignments:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling ManagementFleetApi->list_management_cluster_assignments: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Management cluster identifier (UUIDv7). Bound on &#x60;/v1/management-clusters/{id}&#x60; and &#x60;/v1/management-clusters/{id}/assignments&#x60; for the operator-facing Management Fleet surface.  | 

### Return type

[**ProjectClusterAssignmentList**](ProjectClusterAssignmentList.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | The Project assignments on the cluster. |  -  |
**400** | Malformed cluster id. Body is a &#x60;Problem&#x60; with &#x60;code: invalid_management_cluster_id&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to observe the cluster. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **list_management_clusters**
> ManagementClusterList list_management_clusters()

List the registered management clusters.

Returns the slug-ordered fleet of registered management
clusters. The handler runs an `observe` ReBAC check on the
`managementfleet:fleet` singleton before the read; an
unauthorised caller never observes the fleet's existence or
size. The projection is metadata-only — it carries the cluster
identity, slug, region, and last-observed status, never the
kubeconfig Secret reference's contents.


### Example


```python
import plexsphere
from plexsphere.models.management_cluster_list import ManagementClusterList
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
    api_instance = plexsphere.ManagementFleetApi(api_client)

    try:
        # List the registered management clusters.
        api_response = api_instance.list_management_clusters()
        print("The response of ManagementFleetApi->list_management_clusters:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling ManagementFleetApi->list_management_clusters: %s\n" % e)
```



### Parameters

This endpoint does not need any parameter.

### Return type

[**ManagementClusterList**](ManagementClusterList.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | The registered management fleet. |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to observe the management fleet. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **register_management_cluster**
> ManagementClusterResponse register_management_cluster(register_management_cluster_request)

Register a management cluster into the fleet.

Registers a new management cluster. The handler runs a `manage`
ReBAC check on the `managementfleet:fleet` singleton, then
delegates to the Management Fleet service which mints a UUIDv7
cluster id, constructs the `ManagementCluster` aggregate
through its validating constructor, and persists it.

Registration is the ONLY cluster-creating mutation on the
surface; assignment creation stays reconciliation/broker-only.
A duplicate `slug` surfaces as `409 management_cluster_conflict`
— the slug is the human-facing shard key and is unique across
the fleet.


### Example


```python
import plexsphere
from plexsphere.models.management_cluster_response import ManagementClusterResponse
from plexsphere.models.register_management_cluster_request import RegisterManagementClusterRequest
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
    api_instance = plexsphere.ManagementFleetApi(api_client)
    register_management_cluster_request = {"name":"Production EU","slug":"prod-eu","region":"eu-west","kubeconfig_secret_ref":"plexsphere-mgmt-prod-eu-kubeconfig","status":"active"} # RegisterManagementClusterRequest | 

    try:
        # Register a management cluster into the fleet.
        api_response = api_instance.register_management_cluster(register_management_cluster_request)
        print("The response of ManagementFleetApi->register_management_cluster:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling ManagementFleetApi->register_management_cluster: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **register_management_cluster_request** | [**RegisterManagementClusterRequest**](RegisterManagementClusterRequest.md)|  | 

### Return type

[**ManagementClusterResponse**](ManagementClusterResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**201** | Management cluster registered. |  -  |
**400** | Malformed body. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;invalid_body&#x60;, &#x60;invalid_management_cluster&#x60; }.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to manage the management fleet. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**409** | A management cluster with the same &#x60;slug&#x60; already exists. Body is a &#x60;Problem&#x60; with &#x60;code: management_cluster_conflict&#x60;.  |  -  |
**413** | Request body exceeded the 8 KiB management-fleet ceiling.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **terminate_project_management_cluster_assignment**
> ProjectClusterAssignmentResponse terminate_project_management_cluster_assignment(project_id)

Request teardown of a Project's namespace.

Advances the namespace lifecycle phase of the Project
identified by `{project_id}` to `Terminating`, signalling the
reconciliation loop to tear the namespace down. The handler
runs a `manage` ReBAC check on the `managementfleet:fleet`
singleton.

The call is idempotent: a namespace already `Terminating` or
`Deleted` is returned unchanged with `200`. This is the ONLY
assignment mutation on the operator surface — assignment
creation, re-pointing, and the final unassign remain
reconciliation/broker-driven so the assignment-immutability
invariant cannot be bypassed over HTTP.


### Example


```python
import plexsphere
from plexsphere.models.project_cluster_assignment_response import ProjectClusterAssignmentResponse
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
    api_instance = plexsphere.ManagementFleetApi(api_client)
    project_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Project (UUIDv7). Bound on `/v1/projects/{project_id}/management-cluster-assignment` and its `/terminate` sub-resource — the Management Fleet assignment is keyed by the Project it places. 

    try:
        # Request teardown of a Project's namespace.
        api_response = api_instance.terminate_project_management_cluster_assignment(project_id)
        print("The response of ManagementFleetApi->terminate_project_management_cluster_assignment:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling ManagementFleetApi->terminate_project_management_cluster_assignment: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **project_id** | **UUID**| Owning Project (UUIDv7). Bound on &#x60;/v1/projects/{project_id}/management-cluster-assignment&#x60; and its &#x60;/terminate&#x60; sub-resource — the Management Fleet assignment is keyed by the Project it places.  | 

### Return type

[**ProjectClusterAssignmentResponse**](ProjectClusterAssignmentResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Teardown requested (or already in progress — the call is idempotent). Body is the assignment with &#x60;namespace_phase&#x60; advanced to &#x60;Terminating&#x60; (or its unchanged terminal phase).  |  -  |
**400** | Malformed Project id. Body is a &#x60;Problem&#x60; with &#x60;code: invalid_project_id&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to manage the management fleet. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | The Project has no management-cluster assignment. Body is a &#x60;Problem&#x60; with &#x60;code: management_assignment_not_found&#x60;.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

