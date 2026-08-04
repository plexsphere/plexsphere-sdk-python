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

Approves the queue row identified by `{id}`. This is the one
approve entry point for all three queue sources: the handler
reads the row, then dispatches on its `kind` to the owning
application service. The gate is the one the kind's source
surface owns: `approve` on the owning Domain for an `approval`
row, `assign` on the Cloud Credential for a
`credential_assignment` row, `assign` on the Cloud for a
`cloud_assignment` row.

Approval is only legal from the `pending-approval` state (or the
`proposed` state under the empty-policy short-circuit) — any
other source state returns `409 illegal_transition`. On an
assignment row `pending-approval` is the wire presentation of
the stored `requested` state, so the same rule holds. The caller
may not approve a proposal they themselves raised; that
self-approval is rejected with `403 self_approval_denied`.


### Example

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

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

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Approval approved. Body is the metadata projection with &#x60;state: approved&#x60;.  |  -  |
**400** | Malformed id. Body is a &#x60;Problem&#x60; with &#x60;code: invalid_approval_id&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to decide a row of this kind (&#x60;approve&#x60; on the owning Domain, or &#x60;assign&#x60; on the Cloud Credential or Cloud the assignment spends), or the caller is the proposer of this proposal and may not approve their own request (body is a &#x60;Problem&#x60; with &#x60;code: self_approval_denied&#x60;).  |  -  |
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

Break-glass covers the `approval` kind only. The emergency
override is a Domain `ApprovalPolicy` construct and the
assignment sources have no counterpart, so a
`credential_assignment` or `cloud_assignment` row answers
`409 illegal_transition`. Decide those rows through
`ApproveApproval` or `RejectApproval`.


### Example

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

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

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

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
**409** | The proposal is not in a state from which the override is legal, or the row is of an assignment kind, which has no emergency override. Body is a &#x60;Problem&#x60; with &#x60;code: illegal_transition&#x60;.  |  -  |
**413** | Request body exceeded the 8 KiB ceiling. Body is a &#x60;Problem&#x60; with &#x60;code: request_body_too_large&#x60;.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **get_approval**
> Approval get_approval(id)

Inspect a single Approval.

Returns the lifecycle metadata projection of the queue row
identified by `{id}`. The row may come from any of the three
queue sources, and its `kind` names which one.

The visibility check is the same per-row, fail-closed check
`ListApprovals` applies, resolved once the row read has revealed
the kind: `read` on the owning Domain for an `approval` row,
`assign` on the Cloud Credential for a `credential_assignment`
row, `assign` on the Cloud for a `cloud_assignment` row.

An assignment row presents its stored `requested` state as the
wire state `pending-approval`, and carries `project_id` and
`materialised` in place of the `expires_at` deadline an
`approval` row carries.


### Example

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

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

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

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
> ApprovalList list_approvals(status=status, kind=kind, domain_id=domain_id, cloud_id=cloud_id, cursor=cursor, limit=limit)

List dual-control Approvals.

Returns a creation-ordered page of approval-queue rows. The
queue is the union of three sources: generic Approvals,
Credential Assignments, and Cloud Assignments. Every row carries
a `kind` naming its source. The page is optionally narrowed to a
single lifecycle `status`, a single owning `domain_id`, a single
source `kind`, and a single `cloud_id`.

There is no top-level ReBAC gate on this operation: any
authenticated principal may call it, and per-row fail-closed
checks decide what the page contains. An `approval` row is
visible to a caller holding `read` on its owning Domain; an
assignment row is visible to a caller holding `assign` on the
object it spends, the Cloud Credential for a
`credential_assignment` and the Cloud for a `cloud_assignment`.
A row whose check errors or denies is dropped from the page, so
the `items` array is the subset the caller is authorised to see.

The projection carries the row identity and kind, the owning
Domain, the consuming Project on assignment rows, the proposer,
the proposed action and target, the lifecycle state, the
decision metadata once a terminal state is reached, and the
names-only audit caveat projection. An assignment row presents
its stored `requested` state as the wire state
`pending-approval` so a client filters the whole queue with one
vocabulary.

The pagination cursor is HMAC-signed and bound to the
per-(caller, pepper) pseudonym, so a cursor minted by one
principal cannot be replayed by another — the cross-caller
replay surfaces as `403 cursor_binding_mismatch`. A tampered
envelope or unknown version byte stays on `400 invalid_cursor`.


### Example

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

```python
import plexsphere
from plexsphere.models.approval_kind import ApprovalKind
from plexsphere.models.approval_list import ApprovalList
from plexsphere.models.approval_state import ApprovalState
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
    api_instance = plexsphere.ApprovalsApi(api_client)
    status = plexsphere.ApprovalState() # ApprovalState | Optional lifecycle filter. When present, only rows in the named state are returned. A value outside the vocabulary is rejected with `400 invalid_status`.  (optional)
    kind = plexsphere.ApprovalKind() # ApprovalKind | Optional source filter. When present, only rows of the named source family are returned. A value outside the vocabulary is rejected with `400 invalid_kind`.  (optional)
    domain_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Optional owning-Domain filter. When present, only rows belonging to the named Domain are returned.  (optional)
    cloud_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Optional Cloud filter. When present, only assignment rows that target the named Cloud are returned: a `cloud_assignment` row for that Cloud, or a `credential_assignment` row for any Cloud Credential the Cloud owns. An `approval` row never matches this filter, so combining `cloud_id` with `kind=approval` returns an empty page. A malformed value is rejected with `400 invalid_cloud_id`.  (optional)
    cursor = 'cursor_example' # str | Opaque continuation token returned by a previous call's `next_cursor`. The encoding is HMAC-signed by the server so a tampered cursor surfaces as `400`.  (optional)
    limit = 50 # int | Maximum number of items to return in a single page. A value outside [1, 200] is rejected with a `400` Problem rather than silently clamped.  (optional) (default to 50)

    try:
        # List dual-control Approvals.
        api_response = api_instance.list_approvals(status=status, kind=kind, domain_id=domain_id, cloud_id=cloud_id, cursor=cursor, limit=limit)
        print("The response of ApprovalsApi->list_approvals:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling ApprovalsApi->list_approvals: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **status** | [**ApprovalState**](.md)| Optional lifecycle filter. When present, only rows in the named state are returned. A value outside the vocabulary is rejected with &#x60;400 invalid_status&#x60;.  | [optional] 
 **kind** | [**ApprovalKind**](.md)| Optional source filter. When present, only rows of the named source family are returned. A value outside the vocabulary is rejected with &#x60;400 invalid_kind&#x60;.  | [optional] 
 **domain_id** | **UUID**| Optional owning-Domain filter. When present, only rows belonging to the named Domain are returned.  | [optional] 
 **cloud_id** | **UUID**| Optional Cloud filter. When present, only assignment rows that target the named Cloud are returned: a &#x60;cloud_assignment&#x60; row for that Cloud, or a &#x60;credential_assignment&#x60; row for any Cloud Credential the Cloud owns. An &#x60;approval&#x60; row never matches this filter, so combining &#x60;cloud_id&#x60; with &#x60;kind&#x3D;approval&#x60; returns an empty page. A malformed value is rejected with &#x60;400 invalid_cloud_id&#x60;.  | [optional] 
 **cursor** | **str**| Opaque continuation token returned by a previous call&#39;s &#x60;next_cursor&#x60;. The encoding is HMAC-signed by the server so a tampered cursor surfaces as &#x60;400&#x60;.  | [optional] 
 **limit** | **int**| Maximum number of items to return in a single page. A value outside [1, 200] is rejected with a &#x60;400&#x60; Problem rather than silently clamped.  | [optional] [default to 50]

### Return type

[**ApprovalList**](ApprovalList.md)

### Authorization

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Page of Approvals. |  -  |
**400** | Invalid query parameters — typically a tampered or malformed cursor, an out-of-range &#x60;limit&#x60;, a malformed &#x60;domain_id&#x60; or &#x60;cloud_id&#x60; filter, or a &#x60;status&#x60; / &#x60;kind&#x60; value outside its vocabulary. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;invalid_cursor&#x60;, &#x60;invalid_limit&#x60;, &#x60;invalid_domain_id&#x60;, &#x60;invalid_cloud_id&#x60;, &#x60;invalid_status&#x60;, &#x60;invalid_kind&#x60; }.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | The pagination cursor was minted by a different caller and the per-(caller, pepper) HMAC binding rejected the replay. Body is a &#x60;Problem&#x60; with &#x60;code &#x3D; cursor_binding_mismatch&#x60;. Lacking permission is NOT a &#x60;403&#x60; here: the operation has no top-level gate, and a caller authorised for nothing receives &#x60;200&#x60; with an empty &#x60;items&#x60; array.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **reject_approval**
> Approval reject_approval(id, reject_approval_request)

Reject a pending Approval.

Rejects the queue row identified by `{id}`. This is the one
reject entry point for all three queue sources: the handler
reads the row, then dispatches on its `kind` to the owning
application service, which moves the row to the `rejected` state
and appends the rejection outbox event in a single transaction.
The gate is the one the kind's source surface owns: `approve` on
the owning Domain for an `approval` row, `assign` on the Cloud
Credential for a `credential_assignment` row, `assign` on the
Cloud for a `cloud_assignment` row. The `reason` from the body
is recorded on the decision as an operator-supplied audit
string.

Rejection is only legal from the `pending-approval` state — any
other source state returns `409 illegal_transition`. On an
assignment row `pending-approval` is the wire presentation of
the stored `requested` state, so the same rule holds.


### Example

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

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

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Approval rejected. Body is the metadata projection with &#x60;state: rejected&#x60;.  |  -  |
**400** | Malformed id or body. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;invalid_approval_id&#x60;, &#x60;invalid_body&#x60;, &#x60;invalid_decision_reason&#x60; }.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to decide a row of this kind (&#x60;approve&#x60; on the owning Domain, or &#x60;assign&#x60; on the Cloud Credential or Cloud the assignment spends). Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Approval not found. Body is a &#x60;Problem&#x60; with &#x60;code: approval_not_found&#x60;.  |  -  |
**409** | The proposal is not in a state from which rejection is legal. Body is a &#x60;Problem&#x60; with &#x60;code: illegal_transition&#x60;.  |  -  |
**413** | Request body exceeded the 8 KiB ceiling. Body is a &#x60;Problem&#x60; with &#x60;code: request_body_too_large&#x60;.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

