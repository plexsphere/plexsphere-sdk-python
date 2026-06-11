# plexsphere.ActionsApi

All URIs are relative to *http://localhost*

Method | HTTP request | Description
------------- | ------------- | -------------
[**dispatch_execution**](ActionsApi.md#dispatch_execution) | **POST** /v1/projects/{project_id}/executions:dispatch | Dispatch an action to a single Node or a label-selected cohort.
[**get_execution**](ActionsApi.md#get_execution) | **GET** /v1/projects/{project_id}/executions/{execution_id} | Inspect a single action Execution.
[**list_executions**](ActionsApi.md#list_executions) | **GET** /v1/projects/{project_id}/executions | List action Executions for a Project.
[**post_node_execution_callback**](ActionsApi.md#post_node_execution_callback) | **POST** /v1/nodes/{id}/executions/{exec_id} | Report an action-execution status advance from a Node.


# **dispatch_execution**
> Execution dispatch_execution(project_id, dispatch_execution_request)

Dispatch an action to a single Node or a label-selected cohort.

Dispatches a named action — a built-in shipped with the Node
agent or a user-declared hook — to a target set inside the
owning Project. The handler runs the dispatch ReBAC check on the
Project BEFORE any persistence write, resolves the target cohort
(either a single `node_id` or the Nodes matching an opaque label
`selector`), gates the dispatch on capability availability and
hook integrity, then mints one Execution that fans out to one
per-Node invocation. Each invocation carries a unique callback
URL under `/v1/nodes/{id}/executions/{exec_id}` the Node reports
progress back to.

The request body supplies the `action` name, the action `type`
(`builtin` or `hook`), an opaque `parameters` JSON document, and
an optional `timeout_seconds`. The target is EXACTLY ONE of a
single `node_id` or an opaque label `selector` — supplying both,
or neither, is rejected.


### Example


```python
import plexsphere
from plexsphere.models.dispatch_execution_request import DispatchExecutionRequest
from plexsphere.models.execution import Execution
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
    api_instance = plexsphere.ActionsApi(api_client)
    project_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Project. The Action Orchestrator dispatch and read operations are scoped per Project, so every operator-facing endpoint requires the Project identifier in the path. 
    dispatch_execution_request = {"action":"restart-agent","type":"builtin","parameters":{},"timeout_seconds":300,"selector":"role=worker,region=eu-west"} # DispatchExecutionRequest | 

    try:
        # Dispatch an action to a single Node or a label-selected cohort.
        api_response = api_instance.dispatch_execution(project_id, dispatch_execution_request)
        print("The response of ActionsApi->dispatch_execution:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling ActionsApi->dispatch_execution: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **project_id** | **UUID**| Owning Project. The Action Orchestrator dispatch and read operations are scoped per Project, so every operator-facing endpoint requires the Project identifier in the path.  | 
 **dispatch_execution_request** | [**DispatchExecutionRequest**](DispatchExecutionRequest.md)|  | 

### Return type

[**Execution**](Execution.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**201** | Execution dispatched. Body is the metadata-only Execution projection with one target per resolved Node.  |  -  |
**400** | Invalid request — typically a malformed body, a malformed label selector (&#x60;code: malformed_selector&#x60;), an action the target has not declared (&#x60;code: action_not_declared&#x60;), or a target specification that is not exactly one of &#x60;node_id&#x60; or &#x60;selector&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to dispatch actions in the owning Project. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | The owning Project does not exist or is not visible. Body is a &#x60;Problem&#x60; with &#x60;code: project_not_found&#x60;.  |  -  |
**409** | The dispatch targets a hook that failed its trust-on-first-use integrity check — the Node&#39;s declared hook digest drifted from the pinned known-good baseline, or no baseline has ever been catalogued for it (the gate fails closed). Body is a &#x60;Problem&#x60; with &#x60;code: hook_integrity_violation&#x60;.  |  -  |
**422** | The dispatch could not be admitted because the resolved target cohort is empty. Body is a &#x60;Problem&#x60; with &#x60;code: selector_empty_cohort&#x60; — the label selector matched no Node, or every matched Node was dropped by the per-node domain / ReBAC / capability filter.  |  -  |
**429** | The dispatch could not be admitted because admitting one more Execution would breach the per-Domain live-execution cap. Body is a &#x60;Problem&#x60; with &#x60;code: capacity_exceeded&#x60; and a &#x60;detail&#x60; naming the per-Domain live-executions dimension.  |  -  |
**501** | The action-dispatch surface is not yet provisioned in this build. The Problem body&#39;s &#x60;code&#x60; field is &#x60;actions_dispatch_not_provisioned&#x60; so log scrapers can alert on the deferred-wiring state. Resolves once the dispatch service and its target-resolution ports are wired into the v1 handler dependency cone.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **get_execution**
> Execution get_execution(project_id, execution_id)

Inspect a single action Execution.

Returns the metadata-only projection of the Execution identified
by `{execution_id}` within the owning Project. The handler runs
the execution-read ReBAC check on the Project BEFORE the
persistence read.


### Example


```python
import plexsphere
from plexsphere.models.execution import Execution
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
    api_instance = plexsphere.ActionsApi(api_client)
    project_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Project. The Action Orchestrator dispatch and read operations are scoped per Project, so every operator-facing endpoint requires the Project identifier in the path. 
    execution_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Execution identifier (UUIDv7). Bound on `/v1/projects/{project_id}/executions/{execution_id}` for the single-Execution read. 

    try:
        # Inspect a single action Execution.
        api_response = api_instance.get_execution(project_id, execution_id)
        print("The response of ActionsApi->get_execution:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling ActionsApi->get_execution: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **project_id** | **UUID**| Owning Project. The Action Orchestrator dispatch and read operations are scoped per Project, so every operator-facing endpoint requires the Project identifier in the path.  | 
 **execution_id** | **UUID**| Execution identifier (UUIDv7). Bound on &#x60;/v1/projects/{project_id}/executions/{execution_id}&#x60; for the single-Execution read.  | 

### Return type

[**Execution**](Execution.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | The requested Execution. |  -  |
**400** | Malformed id. Body is a &#x60;Problem&#x60; with &#x60;code: invalid_execution_id&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to read this Execution. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Execution not found. Body is a &#x60;Problem&#x60; with &#x60;code: execution_not_found&#x60;.  |  -  |
**501** | The action-read surface is not yet provisioned in this build. The Problem body&#39;s &#x60;code&#x60; field is &#x60;actions_dispatch_not_provisioned&#x60; so log scrapers can alert on the deferred-wiring state.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **list_executions**
> ExecutionList list_executions(project_id, status=status, action_name=action_name, cursor=cursor, limit=limit)

List action Executions for a Project.

Returns a creation-ordered page of Execution lifecycle metadata
for the owning Project, optionally narrowed to a single aggregate
`status` and/or a single `action_name`. The handler runs the
execution-read ReBAC check on the Project BEFORE the persistence
read. The projection is metadata-only — it carries the action
identity, the aggregate status, and the per-Node targets, but no
secret or token columns.

The pagination cursor is opaque and HMAC-signed by the server, so
a tampered cursor surfaces as `400` on the next call.


### Example


```python
import plexsphere
from plexsphere.models.execution_list import ExecutionList
from plexsphere.models.execution_status import ExecutionStatus
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
    api_instance = plexsphere.ActionsApi(api_client)
    project_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Project. The Action Orchestrator dispatch and read operations are scoped per Project, so every operator-facing endpoint requires the Project identifier in the path. 
    status = plexsphere.ExecutionStatus() # ExecutionStatus | Optional aggregate-status filter. When present, only Executions whose aggregate status equals the named value are returned.  (optional)
    action_name = 'action_name_example' # str | Optional action-name filter. When present, only Executions for the named action are returned.  (optional)
    cursor = 'cursor_example' # str | Opaque continuation token returned by a previous call's `next_cursor`. The encoding is HMAC-signed by the server so a tampered cursor surfaces as `400`.  (optional)
    limit = 50 # int | Maximum number of items to return in a single page. The handler clamps the value to [1, 200] before forwarding it to the read service.  (optional) (default to 50)

    try:
        # List action Executions for a Project.
        api_response = api_instance.list_executions(project_id, status=status, action_name=action_name, cursor=cursor, limit=limit)
        print("The response of ActionsApi->list_executions:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling ActionsApi->list_executions: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **project_id** | **UUID**| Owning Project. The Action Orchestrator dispatch and read operations are scoped per Project, so every operator-facing endpoint requires the Project identifier in the path.  | 
 **status** | [**ExecutionStatus**](.md)| Optional aggregate-status filter. When present, only Executions whose aggregate status equals the named value are returned.  | [optional] 
 **action_name** | **str**| Optional action-name filter. When present, only Executions for the named action are returned.  | [optional] 
 **cursor** | **str**| Opaque continuation token returned by a previous call&#39;s &#x60;next_cursor&#x60;. The encoding is HMAC-signed by the server so a tampered cursor surfaces as &#x60;400&#x60;.  | [optional] 
 **limit** | **int**| Maximum number of items to return in a single page. The handler clamps the value to [1, 200] before forwarding it to the read service.  | [optional] [default to 50]

### Return type

[**ExecutionList**](ExecutionList.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Page of Executions. |  -  |
**400** | Invalid query parameters — typically a tampered or malformed cursor, an out-of-range &#x60;limit&#x60;, or a malformed &#x60;status&#x60; filter.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to read Executions in the owning Project. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | The owning Project does not exist or is not visible. Body is a &#x60;Problem&#x60; with &#x60;code: project_not_found&#x60;.  |  -  |
**501** | The action-read surface is not yet provisioned in this build. The Problem body&#39;s &#x60;code&#x60; field is &#x60;actions_dispatch_not_provisioned&#x60; so log scrapers can alert on the deferred-wiring state.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **post_node_execution_callback**
> ExecutionCallbackResponse post_node_execution_callback(id, exec_id, authorization, execution_callback_request)

Report an action-execution status advance from a Node.

Accepts a per-Node action-execution result callback from plexd
and drives the addressed invocation through the closed status
state machine. The handler:

  1. Authenticates the caller against the Node Secret Key (NSK)
     plaintext supplied in the `Authorization: Bearer` header,
     rejecting revoked credentials with 401.
  2. Asserts that the NSK belongs to the Node addressed by the
     path `id`, refusing cross-Node use with 403
     `nsk_node_mismatch` so a leaked NSK cannot be replayed
     against a sibling Node's invocation.
  3. Drives the invocation addressed by `{exec_id}` to the
     reported `status` through a server-side compare-and-set
     against the closed state machine
     (`ack → started → (succeeded | failed | cancelled |
     timeout)`). An illegal advance returns 409
     `invalid_state_transition`; a re-post onto an already-settled
     invocation returns 409 `execution_already_terminal`.
  4. Collects the reported output. A bounded inline `output`
     (≤ 16 KiB) is stored in the control plane; an inline body
     over the ceiling is refused with 413
     `inline_output_too_large`. When the Node declares an
     over-ceiling output, the FIRST callback's 200 response
     carries an `output_upload_url` — a presigned object-store
     PUT URL the Node uploads the full body to before sending the
     terminal callback.

DEFERRED-WIRING POSTURE: until the production composition root
supplies the callback service and its NSK validator, every
request to this endpoint returns 501 with `code:
execution_callback_not_provisioned` so log scrapers can alert on
the deferred-wiring state.


### Example


```python
import plexsphere
from plexsphere.models.execution_callback_request import ExecutionCallbackRequest
from plexsphere.models.execution_callback_response import ExecutionCallbackResponse
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
    api_instance = plexsphere.ActionsApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Node identifier (UUIDv7) — the callback scope.
    exec_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Execution identifier (UUIDv7) the callback settles.
    authorization = 'authorization_example' # str | `Bearer <NSK plaintext>` — the per-Node Node Secret Key issued at registration time. The NSK is bound to the Node addressed by the path `id`; a credential belonging to a different Node surfaces as 403 `nsk_node_mismatch`. 
    execution_callback_request = {"status":"succeeded","exit_code":0,"output":{"inline":"aGVsbG8gd29ybGQ="}} # ExecutionCallbackRequest | 

    try:
        # Report an action-execution status advance from a Node.
        api_response = api_instance.post_node_execution_callback(id, exec_id, authorization, execution_callback_request)
        print("The response of ActionsApi->post_node_execution_callback:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling ActionsApi->post_node_execution_callback: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Node identifier (UUIDv7) — the callback scope. | 
 **exec_id** | **UUID**| Execution identifier (UUIDv7) the callback settles. | 
 **authorization** | **str**| &#x60;Bearer &lt;NSK plaintext&gt;&#x60; — the per-Node Node Secret Key issued at registration time. The NSK is bound to the Node addressed by the path &#x60;id&#x60;; a credential belonging to a different Node surfaces as 403 &#x60;nsk_node_mismatch&#x60;.  | 
 **execution_callback_request** | [**ExecutionCallbackRequest**](ExecutionCallbackRequest.md)|  | 

### Return type

[**ExecutionCallbackResponse**](ExecutionCallbackResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Callback accepted. The invocation has been advanced and the response carries the new status. On the first over-ceiling callback it also carries the presigned object-store PUT URL the Node uploads the full output to.  |  -  |
**400** | Callback body failed structural validation — an invalid JSON envelope or a &#x60;status&#x60; that is not a member of the closed status set.  |  -  |
**401** | NSK in the &#x60;Authorization: Bearer&#x60; header is missing, malformed, or has been revoked. The Problem body&#39;s &#x60;code&#x60; field is &#x60;nsk_revoked&#x60; so log scrapers can distinguish credential revocation from generic auth failures.  |  -  |
**403** | The NSK in the &#x60;Authorization: Bearer&#x60; header authenticates successfully but belongs to a different Node than the path &#x60;id&#x60;. The PermissionDenied body&#39;s &#x60;code&#x60; field is &#x60;nsk_node_mismatch&#x60; so a leaked NSK cannot be replayed against a sibling Node&#39;s invocation.  |  -  |
**404** | No invocation resolves for the addressed &#x60;(id, exec_id)&#x60; pair. Body is a &#x60;Problem&#x60; with &#x60;code: execution_not_found&#x60;.  |  -  |
**409** | The reported status advance is not legal. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;invalid_state_transition&#x60; (the advance is not a legal edge of the state machine), &#x60;execution_already_terminal&#x60; (the invocation has already settled into a terminal status) }.  |  -  |
**413** | The reported inline &#x60;output&#x60; exceeds the 16 KiB control-plane ceiling. Body is a &#x60;Problem&#x60; with &#x60;code: inline_output_too_large&#x60;. Over-ceiling output must be uploaded to the presigned object-store URL returned on the first callback instead.  |  -  |
**501** | The execution-callback surface is not yet provisioned in this build. The Problem body&#39;s &#x60;code&#x60; field is &#x60;execution_callback_not_provisioned&#x60; so log scrapers can alert on the deferred-wiring state. Resolves once the callback service and its NSK validator are wired into the v1 handler dependency cone.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

