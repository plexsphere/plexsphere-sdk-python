# plexsphere.NodesApi

All URIs are relative to *http://localhost*

Method | HTTP request | Description
------------- | ------------- | -------------
[**list_nodes**](NodesApi.md#list_nodes) | **GET** /v1/nodes | List Node aggregates the cluster knows about.


# **list_nodes**
> NodeList list_nodes(cursor=cursor, limit=limit, domain_id=domain_id)

List Node aggregates the cluster knows about.

Returns a page of Node aggregates (VM / Bridge / Worker) the
caller is authorised to see. Per-row visibility is layered on
top of the page: rows the caller cannot `read` are filtered
out so the response items are a subset of the persistence-
level page. The optional `domain_id` query parameter scopes
the page to a single parent Domain. Cursor pagination mirrors
`/v1/projects` exactly.

The pagination cursor is HMAC-signed and bound to the
per-(caller, pepper) pseudonym, so a cursor minted by one
principal cannot be replayed by another — the cross-caller
replay surfaces as `403 cursor_binding_mismatch`. A tampered
envelope or unknown version byte stays on `400 invalid_cursor`.


### Example


```python
import plexsphere
from plexsphere.models.node_list import NodeList
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
    api_instance = plexsphere.NodesApi(api_client)
    cursor = 'cursor_example' # str | Opaque continuation token returned by a previous call's `next_cursor`. The encoding is HMAC-signed by the server so a tampered cursor surfaces as `400`.  (optional)
    limit = 50 # int | Maximum number of items to return in a single page. The handler clamps the value to [1, 200] before forwarding it to the service.  (optional) (default to 50)
    domain_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Optional filter scoping the page to a single parent Domain. Omit to page across every Domain the caller is authorised to see.  (optional)

    try:
        # List Node aggregates the cluster knows about.
        api_response = api_instance.list_nodes(cursor=cursor, limit=limit, domain_id=domain_id)
        print("The response of NodesApi->list_nodes:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling NodesApi->list_nodes: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **cursor** | **str**| Opaque continuation token returned by a previous call&#39;s &#x60;next_cursor&#x60;. The encoding is HMAC-signed by the server so a tampered cursor surfaces as &#x60;400&#x60;.  | [optional] 
 **limit** | **int**| Maximum number of items to return in a single page. The handler clamps the value to [1, 200] before forwarding it to the service.  | [optional] [default to 50]
 **domain_id** | **UUID**| Optional filter scoping the page to a single parent Domain. Omit to page across every Domain the caller is authorised to see.  | [optional] 

### Return type

[**NodeList**](NodeList.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Page of Nodes. |  -  |
**400** | Invalid query parameters — typically a tampered or malformed cursor, an out-of-range limit, or a malformed &#x60;domain_id&#x60; filter.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to list Nodes (body is a &#x60;PermissionDenied&#x60; problem) OR the pagination cursor was minted by a different caller and the per-(caller, pepper) HMAC binding rejected the replay (body is a &#x60;Problem&#x60; with &#x60;code &#x3D; cursor_binding_mismatch&#x60;).  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

