# plexsphere.PolicyApi

All URIs are relative to *http://localhost*

Method | HTTP request | Description
------------- | ------------- | -------------
[**create_policy**](PolicyApi.md#create_policy) | **POST** /v1/projects/{project_id}/policies | Create a Policy in a Project.
[**delete_policy**](PolicyApi.md#delete_policy) | **DELETE** /v1/projects/{project_id}/policies/{policy_id} | Delete a Policy and cascade its revisions.
[**diff_policy_revisions**](PolicyApi.md#diff_policy_revisions) | **GET** /v1/projects/{project_id}/policies/{policy_id}/diff | Compute the diff between two Policy revisions.
[**dry_run_policy**](PolicyApi.md#dry_run_policy) | **POST** /v1/projects/{project_id}/policies/{policy_id}/dry-run | Evaluate a candidate Policy revision without persisting.
[**get_policy**](PolicyApi.md#get_policy) | **GET** /v1/projects/{project_id}/policies/{policy_id} | Fetch a Policy with its head revision.
[**get_policy_revision**](PolicyApi.md#get_policy_revision) | **GET** /v1/projects/{project_id}/policies/{policy_id}/revisions/{revision_id} | Fetch a historical Policy revision.
[**list_policies**](PolicyApi.md#list_policies) | **GET** /v1/projects/{project_id}/policies | List Policies in a Project.
[**list_policy_revisions**](PolicyApi.md#list_policy_revisions) | **GET** /v1/projects/{project_id}/policies/{policy_id}/revisions | List a Policy&#39;s revision history.
[**update_policy**](PolicyApi.md#update_policy) | **PATCH** /v1/projects/{project_id}/policies/{policy_id} | Update a Policy by landing a new revision.


# **create_policy**
> Policy create_policy(project_id, policy_create_request)

Create a Policy in a Project.

Creates a Policy and its initial revision in one transaction.
The handler performs the dual ReBAC check (`policy#manage` on
the to-be-created Policy through its parent Project, plus
`project#operator` on the Project itself) and emits a single
audit entry on the outcome. A duplicate `(project_id, slug)`
surfaces as `409 policy_slug_taken`.


### Example


```python
import plexsphere
from plexsphere.models.policy import Policy
from plexsphere.models.policy_create_request import PolicyCreateRequest
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
    api_instance = plexsphere.PolicyApi(api_client)
    project_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Project (UUIDv7). Bound on every `/v1/projects/{project_id}/policies/...` operation — the Policy aggregate is per-Project and the dual ReBAC check uses the Project as the second relation target. 
    policy_create_request = plexsphere.PolicyCreateRequest() # PolicyCreateRequest | 

    try:
        # Create a Policy in a Project.
        api_response = api_instance.create_policy(project_id, policy_create_request)
        print("The response of PolicyApi->create_policy:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling PolicyApi->create_policy: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **project_id** | **UUID**| Owning Project (UUIDv7). Bound on every &#x60;/v1/projects/{project_id}/policies/...&#x60; operation — the Policy aggregate is per-Project and the dual ReBAC check uses the Project as the second relation target.  | 
 **policy_create_request** | [**PolicyCreateRequest**](PolicyCreateRequest.md)|  | 

### Return type

[**Policy**](Policy.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**201** | Policy created. The &#x60;Location&#x60; header points at the new aggregate&#39;s canonical resource.  |  * Location - Absolute path of the issued Session for the single-Session read.  <br>  |
**400** | Invalid body. |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller failed the dual ReBAC check. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Project not found. |  -  |
**409** | A Policy with the same &#x60;(project_id, slug)&#x60; already exists.  |  -  |
**422** | Aggregate invariant violated — unknown action, unknown protocol, CIDR family mismatch, inverted port range, empty selector, or rule-count out of range.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **delete_policy**
> delete_policy(project_id, policy_id)

Delete a Policy and cascade its revisions.

Deletes the Policy aggregate. Revision children cascade by
foreign-key. The handler performs the dual ReBAC check
(`policy#manage` on the Policy plus `project#operator` on the
parent Project) and emits a single audit entry plus the
`policy_deleted` outbox event on the outcome.


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
    api_instance = plexsphere.PolicyApi(api_client)
    project_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Project (UUIDv7). Bound on every `/v1/projects/{project_id}/policies/...` operation — the Policy aggregate is per-Project and the dual ReBAC check uses the Project as the second relation target. 
    policy_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Policy identifier (UUIDv7). Bound on every `/v1/projects/{project_id}/policies/{policy_id}/...` operation. 

    try:
        # Delete a Policy and cascade its revisions.
        api_instance.delete_policy(project_id, policy_id)
    except Exception as e:
        print("Exception when calling PolicyApi->delete_policy: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **project_id** | **UUID**| Owning Project (UUIDv7). Bound on every &#x60;/v1/projects/{project_id}/policies/...&#x60; operation — the Policy aggregate is per-Project and the dual ReBAC check uses the Project as the second relation target.  | 
 **policy_id** | **UUID**| Policy identifier (UUIDv7). Bound on every &#x60;/v1/projects/{project_id}/policies/{policy_id}/...&#x60; operation.  | 

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
**204** | Policy deleted. |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller failed the dual ReBAC check. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Policy not found or not visible. |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **diff_policy_revisions**
> PolicyDiff diff_policy_revisions(project_id, policy_id, from_revision, to_revision)

Compute the diff between two Policy revisions.

Returns the structural difference between the rule lists of
the two named revisions after canonicalisation, so a
reorder-only edit produces empty arrays. Both `from_revision`
and `to_revision` MUST belong to the addressed Policy; an
out-of-Policy identifier surfaces as `404
revision_not_visible`. The handler gates on `policy#read`.


### Example


```python
import plexsphere
from plexsphere.models.policy_diff import PolicyDiff
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
    api_instance = plexsphere.PolicyApi(api_client)
    project_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Project (UUIDv7). Bound on every `/v1/projects/{project_id}/policies/...` operation — the Policy aggregate is per-Project and the dual ReBAC check uses the Project as the second relation target. 
    policy_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Policy identifier (UUIDv7). Bound on every `/v1/projects/{project_id}/policies/{policy_id}/...` operation. 
    from_revision = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Identifier of the source revision.
    to_revision = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Identifier of the target revision.

    try:
        # Compute the diff between two Policy revisions.
        api_response = api_instance.diff_policy_revisions(project_id, policy_id, from_revision, to_revision)
        print("The response of PolicyApi->diff_policy_revisions:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling PolicyApi->diff_policy_revisions: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **project_id** | **UUID**| Owning Project (UUIDv7). Bound on every &#x60;/v1/projects/{project_id}/policies/...&#x60; operation — the Policy aggregate is per-Project and the dual ReBAC check uses the Project as the second relation target.  | 
 **policy_id** | **UUID**| Policy identifier (UUIDv7). Bound on every &#x60;/v1/projects/{project_id}/policies/{policy_id}/...&#x60; operation.  | 
 **from_revision** | **UUID**| Identifier of the source revision. | 
 **to_revision** | **UUID**| Identifier of the target revision. | 

### Return type

[**PolicyDiff**](PolicyDiff.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Diff between the two revisions. |  -  |
**400** | Invalid query parameters (missing or malformed &#x60;from_revision&#x60; / &#x60;to_revision&#x60;).  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks &#x60;policy#read&#x60;. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Policy not visible or one of the revisions does not belong to this Policy.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **dry_run_policy**
> PolicyDryRunResponse dry_run_policy(project_id, policy_id, policy_dry_run_request)

Evaluate a candidate Policy revision without persisting.

Evaluates a candidate selector + rule list against the
Project's current state and returns the matched Nodes, the
structural diff against the Policy's current head, the count
of peer pairs whose verdict would flip, and any matched Nodes
the caller cannot read. The handler gates on `policy#edit`.
Evaluation is read-only — no rows are written, no outbox
events are emitted, and no audit entry is appended for the
evaluation itself.


### Example


```python
import plexsphere
from plexsphere.models.policy_dry_run_request import PolicyDryRunRequest
from plexsphere.models.policy_dry_run_response import PolicyDryRunResponse
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
    api_instance = plexsphere.PolicyApi(api_client)
    project_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Project (UUIDv7). Bound on every `/v1/projects/{project_id}/policies/...` operation — the Policy aggregate is per-Project and the dual ReBAC check uses the Project as the second relation target. 
    policy_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Policy identifier (UUIDv7). Bound on every `/v1/projects/{project_id}/policies/{policy_id}/...` operation. 
    policy_dry_run_request = plexsphere.PolicyDryRunRequest() # PolicyDryRunRequest | 

    try:
        # Evaluate a candidate Policy revision without persisting.
        api_response = api_instance.dry_run_policy(project_id, policy_id, policy_dry_run_request)
        print("The response of PolicyApi->dry_run_policy:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling PolicyApi->dry_run_policy: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **project_id** | **UUID**| Owning Project (UUIDv7). Bound on every &#x60;/v1/projects/{project_id}/policies/...&#x60; operation — the Policy aggregate is per-Project and the dual ReBAC check uses the Project as the second relation target.  | 
 **policy_id** | **UUID**| Policy identifier (UUIDv7). Bound on every &#x60;/v1/projects/{project_id}/policies/{policy_id}/...&#x60; operation.  | 
 **policy_dry_run_request** | [**PolicyDryRunRequest**](PolicyDryRunRequest.md)|  | 

### Return type

[**PolicyDryRunResponse**](PolicyDryRunResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Dry-run evaluation result. |  -  |
**400** | Invalid body. |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks &#x60;policy#edit&#x60;. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Policy not found or not visible. |  -  |
**422** | Aggregate invariant violated in the candidate ruleset. |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **get_policy**
> Policy get_policy(project_id, policy_id)

Fetch a Policy with its head revision.

Returns the Policy and its fully-hydrated head revision (rule
list and selector). The handler gates on `policy#read`.


### Example


```python
import plexsphere
from plexsphere.models.policy import Policy
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
    api_instance = plexsphere.PolicyApi(api_client)
    project_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Project (UUIDv7). Bound on every `/v1/projects/{project_id}/policies/...` operation — the Policy aggregate is per-Project and the dual ReBAC check uses the Project as the second relation target. 
    policy_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Policy identifier (UUIDv7). Bound on every `/v1/projects/{project_id}/policies/{policy_id}/...` operation. 

    try:
        # Fetch a Policy with its head revision.
        api_response = api_instance.get_policy(project_id, policy_id)
        print("The response of PolicyApi->get_policy:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling PolicyApi->get_policy: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **project_id** | **UUID**| Owning Project (UUIDv7). Bound on every &#x60;/v1/projects/{project_id}/policies/...&#x60; operation — the Policy aggregate is per-Project and the dual ReBAC check uses the Project as the second relation target.  | 
 **policy_id** | **UUID**| Policy identifier (UUIDv7). Bound on every &#x60;/v1/projects/{project_id}/policies/{policy_id}/...&#x60; operation.  | 

### Return type

[**Policy**](Policy.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Policy found. |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks &#x60;policy#read&#x60;. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Policy not found or not visible. |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **get_policy_revision**
> PolicyRevision get_policy_revision(project_id, policy_id, revision_id)

Fetch a historical Policy revision.

Returns the named revision with its full rule list. A
`revision_id` that does not belong to the addressed Policy
surfaces as `404 revision_not_visible` to avoid leaking the
existence of revisions across Policy boundaries. The handler
gates on `policy#read`.


### Example


```python
import plexsphere
from plexsphere.models.policy_revision import PolicyRevision
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
    api_instance = plexsphere.PolicyApi(api_client)
    project_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Project (UUIDv7). Bound on every `/v1/projects/{project_id}/policies/...` operation — the Policy aggregate is per-Project and the dual ReBAC check uses the Project as the second relation target. 
    policy_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Policy identifier (UUIDv7). Bound on every `/v1/projects/{project_id}/policies/{policy_id}/...` operation. 
    revision_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Revision identifier (UUIDv7). Bound on `/v1/projects/{project_id}/policies/{policy_id}/revisions/{revision_id}`. Revisions that do not belong to the addressed Policy surface as `404 revision_not_visible`. 

    try:
        # Fetch a historical Policy revision.
        api_response = api_instance.get_policy_revision(project_id, policy_id, revision_id)
        print("The response of PolicyApi->get_policy_revision:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling PolicyApi->get_policy_revision: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **project_id** | **UUID**| Owning Project (UUIDv7). Bound on every &#x60;/v1/projects/{project_id}/policies/...&#x60; operation — the Policy aggregate is per-Project and the dual ReBAC check uses the Project as the second relation target.  | 
 **policy_id** | **UUID**| Policy identifier (UUIDv7). Bound on every &#x60;/v1/projects/{project_id}/policies/{policy_id}/...&#x60; operation.  | 
 **revision_id** | **UUID**| Revision identifier (UUIDv7). Bound on &#x60;/v1/projects/{project_id}/policies/{policy_id}/revisions/{revision_id}&#x60;. Revisions that do not belong to the addressed Policy surface as &#x60;404 revision_not_visible&#x60;.  | 

### Return type

[**PolicyRevision**](PolicyRevision.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Revision found. |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks &#x60;policy#read&#x60;. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Revision not found, not on this Policy, or not visible. |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **list_policies**
> PolicyListResponse list_policies(project_id, cursor=cursor, limit=limit)

List Policies in a Project.

Returns Policies in the named Project in deterministic order
with cursor pagination. Per-row visibility filtering layers on
top of the persistence-level page so `items` is the subset the
caller can read; `next_cursor` always reflects the
persistence-level boundary so the hidden set cannot be sized
from the cursor.


### Example


```python
import plexsphere
from plexsphere.models.policy_list_response import PolicyListResponse
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
    api_instance = plexsphere.PolicyApi(api_client)
    project_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Project (UUIDv7). Bound on every `/v1/projects/{project_id}/policies/...` operation — the Policy aggregate is per-Project and the dual ReBAC check uses the Project as the second relation target. 
    cursor = 'cursor_example' # str | Opaque continuation token returned by a previous call's `next_cursor`.  (optional)
    limit = 50 # int | Maximum number of items to return in a single page. Clamped to 200 server-side.  (optional) (default to 50)

    try:
        # List Policies in a Project.
        api_response = api_instance.list_policies(project_id, cursor=cursor, limit=limit)
        print("The response of PolicyApi->list_policies:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling PolicyApi->list_policies: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **project_id** | **UUID**| Owning Project (UUIDv7). Bound on every &#x60;/v1/projects/{project_id}/policies/...&#x60; operation — the Policy aggregate is per-Project and the dual ReBAC check uses the Project as the second relation target.  | 
 **cursor** | **str**| Opaque continuation token returned by a previous call&#39;s &#x60;next_cursor&#x60;.  | [optional] 
 **limit** | **int**| Maximum number of items to return in a single page. Clamped to 200 server-side.  | [optional] [default to 50]

### Return type

[**PolicyListResponse**](PolicyListResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Page of Policies. |  -  |
**400** | Invalid query parameters. |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to list Policies in the target Project. Body is a &#x60;PermissionDenied&#x60; problem carrying the ReBAC denial &#x60;reason&#x60;, traversed &#x60;relation_path&#x60;, and &#x60;correlation_id&#x60; that pairs with the audit entry.  |  -  |
**404** | Project not found. |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **list_policy_revisions**
> PolicyRevisionListResponse list_policy_revisions(project_id, policy_id, cursor=cursor, limit=limit)

List a Policy's revision history.

Returns the Policy's revisions in descending `created_at`
order (most recent first) with cursor pagination. The handler
gates on `policy#read`.


### Example


```python
import plexsphere
from plexsphere.models.policy_revision_list_response import PolicyRevisionListResponse
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
    api_instance = plexsphere.PolicyApi(api_client)
    project_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Project (UUIDv7). Bound on every `/v1/projects/{project_id}/policies/...` operation — the Policy aggregate is per-Project and the dual ReBAC check uses the Project as the second relation target. 
    policy_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Policy identifier (UUIDv7). Bound on every `/v1/projects/{project_id}/policies/{policy_id}/...` operation. 
    cursor = 'cursor_example' # str | Opaque continuation token returned by a previous call's `next_cursor`.  (optional)
    limit = 50 # int | Maximum number of items to return in a single page. Clamped to 200 server-side.  (optional) (default to 50)

    try:
        # List a Policy's revision history.
        api_response = api_instance.list_policy_revisions(project_id, policy_id, cursor=cursor, limit=limit)
        print("The response of PolicyApi->list_policy_revisions:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling PolicyApi->list_policy_revisions: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **project_id** | **UUID**| Owning Project (UUIDv7). Bound on every &#x60;/v1/projects/{project_id}/policies/...&#x60; operation — the Policy aggregate is per-Project and the dual ReBAC check uses the Project as the second relation target.  | 
 **policy_id** | **UUID**| Policy identifier (UUIDv7). Bound on every &#x60;/v1/projects/{project_id}/policies/{policy_id}/...&#x60; operation.  | 
 **cursor** | **str**| Opaque continuation token returned by a previous call&#39;s &#x60;next_cursor&#x60;.  | [optional] 
 **limit** | **int**| Maximum number of items to return in a single page. Clamped to 200 server-side.  | [optional] [default to 50]

### Return type

[**PolicyRevisionListResponse**](PolicyRevisionListResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Page of revisions. |  -  |
**400** | Invalid query parameters. |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks &#x60;policy#read&#x60;. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Policy not found or not visible. |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **update_policy**
> Policy update_policy(project_id, policy_id, policy_update_request)

Update a Policy by landing a new revision.

Patches the Policy by landing a new revision carrying the
supplied fields (unset fields inherit from the current head).
The handler performs the dual ReBAC check (`policy#manage` on
the Policy plus `project#operator` on the parent Project) and
advances the head pointer atomically. A stale
`expected_revision_id` loses the partial-unique-head race and
surfaces as `409 revision_conflict`.


### Example


```python
import plexsphere
from plexsphere.models.policy import Policy
from plexsphere.models.policy_update_request import PolicyUpdateRequest
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
    api_instance = plexsphere.PolicyApi(api_client)
    project_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Project (UUIDv7). Bound on every `/v1/projects/{project_id}/policies/...` operation — the Policy aggregate is per-Project and the dual ReBAC check uses the Project as the second relation target. 
    policy_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Policy identifier (UUIDv7). Bound on every `/v1/projects/{project_id}/policies/{policy_id}/...` operation. 
    policy_update_request = plexsphere.PolicyUpdateRequest() # PolicyUpdateRequest | 

    try:
        # Update a Policy by landing a new revision.
        api_response = api_instance.update_policy(project_id, policy_id, policy_update_request)
        print("The response of PolicyApi->update_policy:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling PolicyApi->update_policy: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **project_id** | **UUID**| Owning Project (UUIDv7). Bound on every &#x60;/v1/projects/{project_id}/policies/...&#x60; operation — the Policy aggregate is per-Project and the dual ReBAC check uses the Project as the second relation target.  | 
 **policy_id** | **UUID**| Policy identifier (UUIDv7). Bound on every &#x60;/v1/projects/{project_id}/policies/{policy_id}/...&#x60; operation.  | 
 **policy_update_request** | [**PolicyUpdateRequest**](PolicyUpdateRequest.md)|  | 

### Return type

[**Policy**](Policy.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Policy updated. |  -  |
**400** | Invalid body. |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller failed the dual ReBAC check. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Policy not found or not visible. |  -  |
**409** | Concurrent PATCH lost the partial-unique-head race (&#x60;revision_conflict&#x60;). The caller should re-fetch the current head and rebase the edit.  |  -  |
**422** | Aggregate invariant violated. |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

