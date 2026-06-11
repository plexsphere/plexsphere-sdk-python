# plexsphere.IntegrityApi

All URIs are relative to *http://localhost*

Method | HTTP request | Description
------------- | ------------- | -------------
[**acknowledge_integrity_violation**](IntegrityApi.md#acknowledge_integrity_violation) | **POST** /v1/integrity-violations/{id}/acknowledge | Acknowledge an open integrity violation.
[**list_integrity_violations**](IntegrityApi.md#list_integrity_violations) | **GET** /v1/integrity-violations | List integrity violations across Domains.


# **acknowledge_integrity_violation**
> IntegrityViolationRow acknowledge_integrity_violation(id, integrity_violation_acknowledge_request)

Acknowledge an open integrity violation.

Acknowledges the integrity violation identified by `{id}`,
moving it from `open` to `acknowledged` and recording the
operator-supplied `reason` on the triage row. The handler runs
the platform read gate before the transition.

Acknowledgement is an elevated, audited action and MAY be gated
behind a required OIDC ACR: a caller whose session does not
satisfy the configured ACR is challenged with `401` carrying the
`acr_values` it must re-authenticate with, and no state
transition occurs. Acknowledgement is only legal from the `open`
state — any other source state returns `409 illegal_transition`.
A blank or whitespace-only `reason` is rejected with
`400 invalid_acknowledge_reason`.


### Example


```python
import plexsphere
from plexsphere.models.integrity_violation_acknowledge_request import IntegrityViolationAcknowledgeRequest
from plexsphere.models.integrity_violation_row import IntegrityViolationRow
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
    api_instance = plexsphere.IntegrityApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Integrity-violation identifier (UUIDv7). Bound on `/v1/integrity-violations/{id}/acknowledge` for the triage acknowledge surface. 
    integrity_violation_acknowledge_request = {"reason":"Confirmed planned binary upgrade rolled out by the platform on-call"} # IntegrityViolationAcknowledgeRequest | 

    try:
        # Acknowledge an open integrity violation.
        api_response = api_instance.acknowledge_integrity_violation(id, integrity_violation_acknowledge_request)
        print("The response of IntegrityApi->acknowledge_integrity_violation:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling IntegrityApi->acknowledge_integrity_violation: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Integrity-violation identifier (UUIDv7). Bound on &#x60;/v1/integrity-violations/{id}/acknowledge&#x60; for the triage acknowledge surface.  | 
 **integrity_violation_acknowledge_request** | [**IntegrityViolationAcknowledgeRequest**](IntegrityViolationAcknowledgeRequest.md)|  | 

### Return type

[**IntegrityViolationRow**](IntegrityViolationRow.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Integrity violation acknowledged. Body is the updated triage row with &#x60;status: acknowledged&#x60;.  |  -  |
**400** | Malformed id or body. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;invalid_integrity_violation_id&#x60;, &#x60;invalid_body&#x60;, &#x60;invalid_acknowledge_reason&#x60; }.  |  -  |
**401** | Caller is not authenticated, or the caller&#39;s session does not satisfy the required ACR for this elevated action — the body carries the &#x60;acr_values&#x60; the caller must step up to.  |  -  |
**403** | Caller is not authorized to acknowledge integrity violations. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Integrity violation not found. Body is a &#x60;Problem&#x60; with &#x60;code: integrity_violation_not_found&#x60;.  |  -  |
**409** | The violation is not in a state from which acknowledgement is legal. Body is a &#x60;Problem&#x60; with &#x60;code: illegal_transition&#x60;.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **list_integrity_violations**
> IntegrityViolationList list_integrity_violations(domain_id=domain_id, project_id=project_id, node_id=node_id, kind=kind, status=status, cursor=cursor, limit=limit)

List integrity violations across Domains.

Returns a cursor-paginated page of integrity-violation rows the
agents reported, optionally narrowed by owning Domain, Project,
Node, violation `kind`, or lifecycle `status`. The handler runs
the platform read gate before the persistence read, then layers
a per-row visibility filter on the owning Domain so `items` is
the subset the caller is authorised to see.

The pagination cursor is HMAC-signed and bound to the
per-(caller, pepper) pseudonym; a cross-caller replay surfaces
as `403 cursor_binding_mismatch`, and a tampered or malformed
cursor surfaces as `400 invalid_cursor`. `limit` is clamped to
`[1, 200]` rather than rejected.


### Example


```python
import plexsphere
from plexsphere.models.integrity_violation_kind import IntegrityViolationKind
from plexsphere.models.integrity_violation_list import IntegrityViolationList
from plexsphere.models.integrity_violation_status import IntegrityViolationStatus
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
    api_instance = plexsphere.IntegrityApi(api_client)
    domain_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Optional owning-Domain filter. When present, only violations on the named Domain are returned.  (optional)
    project_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Optional Project filter. When present, only violations on Nodes in the named Project are returned.  (optional)
    node_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Optional Node filter. When present, only violations reported by the named Node are returned.  (optional)
    kind = plexsphere.IntegrityViolationKind() # IntegrityViolationKind | Optional violation-kind filter. Composes with the other filters.  (optional)
    status = plexsphere.IntegrityViolationStatus() # IntegrityViolationStatus | Optional lifecycle-status filter. Composes with the other filters.  (optional)
    cursor = 'cursor_example' # str | Opaque continuation token returned by a previous call's `next_cursor`. The encoding is HMAC-signed so a tampered cursor surfaces as `400`.  (optional)
    limit = 50 # int | Maximum number of items to return in a single page. The handler clamps the value to [1, 200] before forwarding it to the read service.  (optional) (default to 50)

    try:
        # List integrity violations across Domains.
        api_response = api_instance.list_integrity_violations(domain_id=domain_id, project_id=project_id, node_id=node_id, kind=kind, status=status, cursor=cursor, limit=limit)
        print("The response of IntegrityApi->list_integrity_violations:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling IntegrityApi->list_integrity_violations: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **domain_id** | **UUID**| Optional owning-Domain filter. When present, only violations on the named Domain are returned.  | [optional] 
 **project_id** | **UUID**| Optional Project filter. When present, only violations on Nodes in the named Project are returned.  | [optional] 
 **node_id** | **UUID**| Optional Node filter. When present, only violations reported by the named Node are returned.  | [optional] 
 **kind** | [**IntegrityViolationKind**](.md)| Optional violation-kind filter. Composes with the other filters.  | [optional] 
 **status** | [**IntegrityViolationStatus**](.md)| Optional lifecycle-status filter. Composes with the other filters.  | [optional] 
 **cursor** | **str**| Opaque continuation token returned by a previous call&#39;s &#x60;next_cursor&#x60;. The encoding is HMAC-signed so a tampered cursor surfaces as &#x60;400&#x60;.  | [optional] 
 **limit** | **int**| Maximum number of items to return in a single page. The handler clamps the value to [1, 200] before forwarding it to the read service.  | [optional] [default to 50]

### Return type

[**IntegrityViolationList**](IntegrityViolationList.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Page of integrity-violation rows. |  -  |
**400** | Invalid query parameters — typically a tampered or malformed cursor, an out-of-range &#x60;limit&#x60;, or a malformed filter.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to read integrity violations (body is a &#x60;PermissionDenied&#x60; problem) OR the pagination cursor was minted by a different caller and the per-(caller, pepper) HMAC binding rejected the replay (body is a &#x60;Problem&#x60; with &#x60;code &#x3D; cursor_binding_mismatch&#x60;).  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

