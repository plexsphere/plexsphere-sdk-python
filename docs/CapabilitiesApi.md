# plexsphere.CapabilitiesApi

All URIs are relative to *http://localhost*

Method | HTTP request | Description
------------- | ------------- | -------------
[**list_capabilities**](CapabilitiesApi.md#list_capabilities) | **GET** /v1/projects/{project_id}/capabilities | List per-Node capability manifests in a Project.


# **list_capabilities**
> CapabilityRowList list_capabilities(project_id, node_id=node_id, status=status, cursor=cursor, limit=limit)

List per-Node capability manifests in a Project.

Returns a cursor-paginated page of per-Node capability rows in
the named Project. Each row pairs a Node's reported agent binary
with a checksum status — `match` when the reported checksum
equals a known-good artifact, `drift` when it differs, and
`unknown` when no known-good artifact is on record. The handler
runs the platform read gate before the persistence read, then
layers a per-row visibility filter on the Domain owning each
Node so `items` is the subset the caller is authorised to see.

The pagination cursor is HMAC-signed and bound to the
per-(caller, pepper) pseudonym; a cross-caller replay surfaces
as `403 cursor_binding_mismatch`, and a tampered or malformed
cursor surfaces as `400 invalid_cursor`. `limit` is clamped to
`[1, 200]` rather than rejected.


### Example


```python
import plexsphere
from plexsphere.models.capability_row_list import CapabilityRowList
from plexsphere.models.capability_status import CapabilityStatus
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
    api_instance = plexsphere.CapabilitiesApi(api_client)
    project_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Project (UUIDv7). The capability inventory is scoped to Nodes in this Project. 
    node_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Optional Node filter. When present, only the capability row for the named Node is returned.  (optional)
    status = plexsphere.CapabilityStatus() # CapabilityStatus | Optional checksum-status filter. When present, only rows in the named status are returned.  (optional)
    cursor = 'cursor_example' # str | Opaque continuation token returned by a previous call's `next_cursor`. The encoding is HMAC-signed so a tampered cursor surfaces as `400`.  (optional)
    limit = 50 # int | Maximum number of items to return in a single page. The handler clamps the value to [1, 200] before forwarding it to the read service.  (optional) (default to 50)

    try:
        # List per-Node capability manifests in a Project.
        api_response = api_instance.list_capabilities(project_id, node_id=node_id, status=status, cursor=cursor, limit=limit)
        print("The response of CapabilitiesApi->list_capabilities:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling CapabilitiesApi->list_capabilities: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **project_id** | **UUID**| Owning Project (UUIDv7). The capability inventory is scoped to Nodes in this Project.  | 
 **node_id** | **UUID**| Optional Node filter. When present, only the capability row for the named Node is returned.  | [optional] 
 **status** | [**CapabilityStatus**](.md)| Optional checksum-status filter. When present, only rows in the named status are returned.  | [optional] 
 **cursor** | **str**| Opaque continuation token returned by a previous call&#39;s &#x60;next_cursor&#x60;. The encoding is HMAC-signed so a tampered cursor surfaces as &#x60;400&#x60;.  | [optional] 
 **limit** | **int**| Maximum number of items to return in a single page. The handler clamps the value to [1, 200] before forwarding it to the read service.  | [optional] [default to 50]

### Return type

[**CapabilityRowList**](CapabilityRowList.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Page of capability rows. |  -  |
**400** | Invalid query parameters — typically a tampered or malformed cursor, an out-of-range &#x60;limit&#x60;, or a malformed &#x60;node_id&#x60; filter.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to read the capability inventory (body is a &#x60;PermissionDenied&#x60; problem) OR the pagination cursor was minted by a different caller and the per-(caller, pepper) HMAC binding rejected the replay (body is a &#x60;Problem&#x60; with &#x60;code &#x3D; cursor_binding_mismatch&#x60;).  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

