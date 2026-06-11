# plexsphere.HooksApi

All URIs are relative to *http://localhost*

Method | HTTP request | Description
------------- | ------------- | -------------
[**list_hooks**](HooksApi.md#list_hooks) | **GET** /v1/hooks | List discovered hooks across Domains.


# **list_hooks**
> HookList list_hooks(domain_id=domain_id, project_id=project_id, image_digest_match=image_digest_match, cursor=cursor, limit=limit)

List discovered hooks across Domains.

Returns a cursor-paginated page of the Kubernetes hook custom
resources the agents discovered in their clusters, optionally
narrowed by owning Domain, Project, or whether the discovered
image digest matches the declared one. The surface is strictly
read-only — plexsphere records what the agent observed and never
writes hook objects back. The handler runs the platform read
gate before the persistence read, then layers a per-row
visibility filter on the owning Domain so `items` is the subset
the caller is authorised to see.

The pagination cursor is HMAC-signed and bound to the
per-(caller, pepper) pseudonym; a cross-caller replay surfaces
as `403 cursor_binding_mismatch`, and a tampered or malformed
cursor surfaces as `400 invalid_cursor`. `limit` is clamped to
`[1, 200]` rather than rejected.


### Example


```python
import plexsphere
from plexsphere.models.hook_list import HookList
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
    api_instance = plexsphere.HooksApi(api_client)
    domain_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Optional owning-Domain filter. When present, only hooks on the named Domain are returned.  (optional)
    project_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Optional Project filter. When present, only hooks on Nodes in the named Project are returned.  (optional)
    image_digest_match = True # bool | Optional drift filter. When `true`, only hooks whose discovered image digest matches the declared digest are returned; when `false`, only mismatches are returned.  (optional)
    cursor = 'cursor_example' # str | Opaque continuation token returned by a previous call's `next_cursor`. The encoding is HMAC-signed so a tampered cursor surfaces as `400`.  (optional)
    limit = 50 # int | Maximum number of items to return in a single page. The handler clamps the value to [1, 200] before forwarding it to the read service.  (optional) (default to 50)

    try:
        # List discovered hooks across Domains.
        api_response = api_instance.list_hooks(domain_id=domain_id, project_id=project_id, image_digest_match=image_digest_match, cursor=cursor, limit=limit)
        print("The response of HooksApi->list_hooks:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling HooksApi->list_hooks: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **domain_id** | **UUID**| Optional owning-Domain filter. When present, only hooks on the named Domain are returned.  | [optional] 
 **project_id** | **UUID**| Optional Project filter. When present, only hooks on Nodes in the named Project are returned.  | [optional] 
 **image_digest_match** | **bool**| Optional drift filter. When &#x60;true&#x60;, only hooks whose discovered image digest matches the declared digest are returned; when &#x60;false&#x60;, only mismatches are returned.  | [optional] 
 **cursor** | **str**| Opaque continuation token returned by a previous call&#39;s &#x60;next_cursor&#x60;. The encoding is HMAC-signed so a tampered cursor surfaces as &#x60;400&#x60;.  | [optional] 
 **limit** | **int**| Maximum number of items to return in a single page. The handler clamps the value to [1, 200] before forwarding it to the read service.  | [optional] [default to 50]

### Return type

[**HookList**](HookList.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Page of discovered hooks. |  -  |
**400** | Invalid query parameters — typically a tampered or malformed cursor, an out-of-range &#x60;limit&#x60;, or a malformed filter.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to read the discovered-hook catalog (body is a &#x60;PermissionDenied&#x60; problem) OR the pagination cursor was minted by a different caller and the per-(caller, pepper) HMAC binding rejected the replay (body is a &#x60;Problem&#x60; with &#x60;code &#x3D; cursor_binding_mismatch&#x60;).  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

