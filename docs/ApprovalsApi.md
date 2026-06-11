# plexsphere.ApprovalsApi

All URIs are relative to *http://localhost*

Method | HTTP request | Description
------------- | ------------- | -------------
[**approve_approval**](ApprovalsApi.md#approve_approval) | **POST** /v1/approvals/{id}/approve | Approve a pending Approval.
[**break_glass_approval**](ApprovalsApi.md#break_glass_approval) | **POST** /v1/approvals/{id}/break-glass | Force a pending Approval via the emergency override.
[**get_approval**](ApprovalsApi.md#get_approval) | **GET** /v1/approvals/{id} | Inspect a single Approval.
[**list_approvals**](ApprovalsApi.md#list_approvals) | **GET** /v1/approvals | List dual-control Approvals.
[**reject_approval**](ApprovalsApi.md#reject_approval) | **POST** /v1/approvals/{id}/reject | Reject a pending Approval.


# **approve_approval**
> Approval approve_approval(id)

Approve a pending Approval.

Approves the Approval identified by `{id}`. The handler reads
the row to resolve the owning Domain, runs the `approve` ReBAC
check on that Domain, then delegates to the approval-workflow
application service which moves the proposal to the `approved`
state and appends the approval outbox event in a single
transaction.

Approval is only legal from the `pending-approval` state (or the
`proposed` state under the empty-policy short-circuit) — any
other source state returns `409 illegal_transition`. The caller
may not approve a proposal they themselves raised; that
self-approval is rejected with `403 self_approval_denied`.


### Example


```python
import plexsphere
from plexsphere.models.approval import Approval
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
    api_instance = plexsphere.ApprovalsApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Approval identifier (UUIDv7). Bound on `/v1/approvals/{id}`, `/v1/approvals/{id}/approve`, `/v1/approvals/{id}/reject`, and `/v1/approvals/{id}/break-glass` for the dual-control approval read + decision surface. 

    try:
        # Approve a pending Approval.
        api_response = api_instance.approve_approval(id)
        print("The response of ApprovalsApi->approve_approval:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling ApprovalsApi->approve_approval: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Approval identifier (UUIDv7). Bound on &#x60;/v1/approvals/{id}&#x60;, &#x60;/v1/approvals/{id}/approve&#x60;, &#x60;/v1/approvals/{id}/reject&#x60;, and &#x60;/v1/approvals/{id}/break-glass&#x60; for the dual-control approval read + decision surface.  | 

### Return type

[**Approval**](Approval.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Approval approved. Body is the metadata projection with &#x60;state: approved&#x60;.  |  -  |
**400** | Malformed id. Body is a &#x60;Problem&#x60; with &#x60;code: invalid_approval_id&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to approve in the owning Domain, or the caller is the proposer of this proposal and may not approve their own request (body is a &#x60;Problem&#x60; with &#x60;code: self_approval_denied&#x60;).  |  -  |
**404** | Approval not found. Body is a &#x60;Problem&#x60; with &#x60;code: approval_not_found&#x60;.  |  -  |
**409** | The proposal is not in a state from which approval is legal. Body is a &#x60;Problem&#x60; with &#x60;code: illegal_transition&#x60;.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **break_glass_approval**
> Approval break_glass_approval(id, break_glass_request)

Force a pending Approval via the emergency override.

Forces the Approval identified by `{id}` to the `approved`
state via the emergency break-glass override, bypassing the
second-party decision. The handler reads the row to resolve the
owning Domain, runs the `emergency_approver` ReBAC check on that
Domain — the ordinary `approve` relation is NOT sufficient —
then delegates to the approval-workflow application service
which moves the proposal to `approved` and appends the
break-glass outbox event in a single transaction.

The override is only legal from the `pending-approval` state —
any other source state returns `409 illegal_transition`. The
mandatory `reason` justifies the emergency and is recorded by
field NAME only on the decision's audit caveat context; the
value itself is PII routed to a PII-safe sink and never crosses
the contract boundary.


### Example


```python
import plexsphere
from plexsphere.models.approval import Approval
from plexsphere.models.break_glass_request import BreakGlassRequest
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
    api_instance = plexsphere.ApprovalsApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Approval identifier (UUIDv7). Bound on `/v1/approvals/{id}`, `/v1/approvals/{id}/approve`, `/v1/approvals/{id}/reject`, and `/v1/approvals/{id}/break-glass` for the dual-control approval read + decision surface. 
    break_glass_request = {"reason":"Production incident INC-4815 requires immediate rotation"} # BreakGlassRequest | 

    try:
        # Force a pending Approval via the emergency override.
        api_response = api_instance.break_glass_approval(id, break_glass_request)
        print("The response of ApprovalsApi->break_glass_approval:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling ApprovalsApi->break_glass_approval: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Approval identifier (UUIDv7). Bound on &#x60;/v1/approvals/{id}&#x60;, &#x60;/v1/approvals/{id}/approve&#x60;, &#x60;/v1/approvals/{id}/reject&#x60;, and &#x60;/v1/approvals/{id}/break-glass&#x60; for the dual-control approval read + decision surface.  | 
 **break_glass_request** | [**BreakGlassRequest**](BreakGlassRequest.md)|  | 

### Return type

[**Approval**](Approval.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Approval forced to approved via the emergency override. Body is the metadata projection with &#x60;state: approved&#x60;.  |  -  |
**400** | Malformed id or body. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;invalid_approval_id&#x60;, &#x60;invalid_body&#x60;, &#x60;invalid_break_glass_reason&#x60; }.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller does not hold the &#x60;emergency_approver&#x60; relation on the owning Domain. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Approval not found. Body is a &#x60;Problem&#x60; with &#x60;code: approval_not_found&#x60;.  |  -  |
**409** | The proposal is not in a state from which the override is legal. Body is a &#x60;Problem&#x60; with &#x60;code: illegal_transition&#x60;.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **get_approval**
> Approval get_approval(id)

Inspect a single Approval.

Returns the lifecycle metadata projection of the Approval
identified by `{id}`. The handler runs the approval-read ReBAC
check on the owning Domain BEFORE the persistence read.


### Example


```python
import plexsphere
from plexsphere.models.approval import Approval
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
    api_instance = plexsphere.ApprovalsApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Approval identifier (UUIDv7). Bound on `/v1/approvals/{id}`, `/v1/approvals/{id}/approve`, `/v1/approvals/{id}/reject`, and `/v1/approvals/{id}/break-glass` for the dual-control approval read + decision surface. 

    try:
        # Inspect a single Approval.
        api_response = api_instance.get_approval(id)
        print("The response of ApprovalsApi->get_approval:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling ApprovalsApi->get_approval: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Approval identifier (UUIDv7). Bound on &#x60;/v1/approvals/{id}&#x60;, &#x60;/v1/approvals/{id}/approve&#x60;, &#x60;/v1/approvals/{id}/reject&#x60;, and &#x60;/v1/approvals/{id}/break-glass&#x60; for the dual-control approval read + decision surface.  | 

### Return type

[**Approval**](Approval.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | The requested Approval. |  -  |
**400** | Malformed id. Body is a &#x60;Problem&#x60; with &#x60;code: invalid_approval_id&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to read this Approval. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Approval not found. Body is a &#x60;Problem&#x60; with &#x60;code: approval_not_found&#x60;.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **list_approvals**
> ApprovalList list_approvals(status=status, domain_id=domain_id, cursor=cursor, limit=limit)

List dual-control Approvals.

Returns a creation-ordered page of Approval lifecycle metadata
across the approval queue, optionally narrowed to a single
lifecycle `status` and/or a single owning `domain_id`. The
handler runs the approval-read ReBAC check BEFORE the
persistence read, then layers a per-row visibility filter on
top so the response items are the subset of the persistence-
level page the caller is authorised to see.

The projection carries the proposal identity, the owning Domain,
the proposer, the proposed action and target, the lifecycle
state, the decision metadata once a terminal state is reached,
and the names-only audit caveat projection.

The pagination cursor is HMAC-signed and bound to the
per-(caller, pepper) pseudonym, so a cursor minted by one
principal cannot be replayed by another — the cross-caller
replay surfaces as `403 cursor_binding_mismatch`. A tampered
envelope or unknown version byte stays on `400 invalid_cursor`.


### Example


```python
import plexsphere
from plexsphere.models.approval_list import ApprovalList
from plexsphere.models.approval_state import ApprovalState
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
    api_instance = plexsphere.ApprovalsApi(api_client)
    status = plexsphere.ApprovalState() # ApprovalState | Optional lifecycle filter. When present, only Approvals in the named state are returned.  (optional)
    domain_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Optional owning-Domain filter. When present, only Approvals belonging to the named Domain are returned.  (optional)
    cursor = 'cursor_example' # str | Opaque continuation token returned by a previous call's `next_cursor`. The encoding is HMAC-signed by the server so a tampered cursor surfaces as `400`.  (optional)
    limit = 50 # int | Maximum number of items to return in a single page. The handler clamps the value to [1, 200] before forwarding it to the read service.  (optional) (default to 50)

    try:
        # List dual-control Approvals.
        api_response = api_instance.list_approvals(status=status, domain_id=domain_id, cursor=cursor, limit=limit)
        print("The response of ApprovalsApi->list_approvals:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling ApprovalsApi->list_approvals: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **status** | [**ApprovalState**](.md)| Optional lifecycle filter. When present, only Approvals in the named state are returned.  | [optional] 
 **domain_id** | **UUID**| Optional owning-Domain filter. When present, only Approvals belonging to the named Domain are returned.  | [optional] 
 **cursor** | **str**| Opaque continuation token returned by a previous call&#39;s &#x60;next_cursor&#x60;. The encoding is HMAC-signed by the server so a tampered cursor surfaces as &#x60;400&#x60;.  | [optional] 
 **limit** | **int**| Maximum number of items to return in a single page. The handler clamps the value to [1, 200] before forwarding it to the read service.  | [optional] [default to 50]

### Return type

[**ApprovalList**](ApprovalList.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Page of Approvals. |  -  |
**400** | Invalid query parameters — typically a tampered or malformed cursor, an out-of-range &#x60;limit&#x60;, or a malformed &#x60;domain_id&#x60; filter.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to read the approval queue (body is a &#x60;PermissionDenied&#x60; problem) OR the pagination cursor was minted by a different caller and the per-(caller, pepper) HMAC binding rejected the replay (body is a &#x60;Problem&#x60; with &#x60;code &#x3D; cursor_binding_mismatch&#x60;).  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **reject_approval**
> Approval reject_approval(id, reject_approval_request)

Reject a pending Approval.

Rejects the Approval identified by `{id}`. The handler reads the
row to resolve the owning Domain, runs the `approve` ReBAC check
on that Domain, then delegates to the approval-workflow
application service which moves the proposal to the `rejected`
state and appends the rejection outbox event in a single
transaction. The `reason` from the body is recorded on the
decision as an operator-supplied audit string.

Rejection is only legal from the `pending-approval` state — any
other source state returns `409 illegal_transition`.


### Example


```python
import plexsphere
from plexsphere.models.approval import Approval
from plexsphere.models.reject_approval_request import RejectApprovalRequest
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
    api_instance = plexsphere.ApprovalsApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Approval identifier (UUIDv7). Bound on `/v1/approvals/{id}`, `/v1/approvals/{id}/approve`, `/v1/approvals/{id}/reject`, and `/v1/approvals/{id}/break-glass` for the dual-control approval read + decision surface. 
    reject_approval_request = {"reason":"Target resource is scheduled for decommission"} # RejectApprovalRequest | 

    try:
        # Reject a pending Approval.
        api_response = api_instance.reject_approval(id, reject_approval_request)
        print("The response of ApprovalsApi->reject_approval:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling ApprovalsApi->reject_approval: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Approval identifier (UUIDv7). Bound on &#x60;/v1/approvals/{id}&#x60;, &#x60;/v1/approvals/{id}/approve&#x60;, &#x60;/v1/approvals/{id}/reject&#x60;, and &#x60;/v1/approvals/{id}/break-glass&#x60; for the dual-control approval read + decision surface.  | 
 **reject_approval_request** | [**RejectApprovalRequest**](RejectApprovalRequest.md)|  | 

### Return type

[**Approval**](Approval.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Approval rejected. Body is the metadata projection with &#x60;state: rejected&#x60;.  |  -  |
**400** | Malformed id or body. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;invalid_approval_id&#x60;, &#x60;invalid_body&#x60;, &#x60;invalid_decision_reason&#x60; }.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to decide in the owning Domain. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Approval not found. Body is a &#x60;Problem&#x60; with &#x60;code: approval_not_found&#x60;.  |  -  |
**409** | The proposal is not in a state from which rejection is legal. Body is a &#x60;Problem&#x60; with &#x60;code: illegal_transition&#x60;.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

