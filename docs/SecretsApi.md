# plexsphere.SecretsApi

All URIs are relative to *http://localhost*

Method | HTTP request | Description
------------- | ------------- | -------------
[**list_project_secrets**](SecretsApi.md#list_project_secrets) | **GET** /v1/projects/{project_id}/secrets | List the secret names and current versions a Project exposes.


# **list_project_secrets**
> ProjectSecretList list_project_secrets(project_id, cursor=cursor, limit=limit)

List the secret names and current versions a Project exposes.

Returns a name-ordered page of Secret Store inventory metadata for
the owning Project — each entry carrying ONLY the secret `name` and
its `current_version`. The handler runs the secret-read ReBAC check
on the Project BEFORE the persistence read. The projection is
metadata-only: it never carries a secret payload, an OpenBao path,
or any wrapping material, so an operator can audit what a Project
exposes without ever seeing a payload.

The pagination cursor is opaque and HMAC-signed by the server, so a
tampered cursor surfaces as `400` on the next call.


### Example

* Bearer (JWT) Authentication (operatorBearer):

```python
import plexsphere
from plexsphere.models.project_secret_list import ProjectSecretList
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

# Enter a context with an instance of the API client
with plexsphere.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = plexsphere.SecretsApi(api_client)
    project_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Project. The operator-facing Secret Store inventory is scoped per Project, so the listing endpoint requires the Project identifier in the path. 
    cursor = 'cursor_example' # str | Opaque continuation token returned by a previous call's `next_cursor`. The encoding is HMAC-signed by the server so a tampered cursor surfaces as `400`.  (optional)
    limit = 50 # int | Maximum number of items to return in a single page. The handler clamps the value to [1, 200] before forwarding it to the read service.  (optional) (default to 50)

    try:
        # List the secret names and current versions a Project exposes.
        api_response = api_instance.list_project_secrets(project_id, cursor=cursor, limit=limit)
        print("The response of SecretsApi->list_project_secrets:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling SecretsApi->list_project_secrets: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **project_id** | **UUID**| Owning Project. The operator-facing Secret Store inventory is scoped per Project, so the listing endpoint requires the Project identifier in the path.  | 
 **cursor** | **str**| Opaque continuation token returned by a previous call&#39;s &#x60;next_cursor&#x60;. The encoding is HMAC-signed by the server so a tampered cursor surfaces as &#x60;400&#x60;.  | [optional] 
 **limit** | **int**| Maximum number of items to return in a single page. The handler clamps the value to [1, 200] before forwarding it to the read service.  | [optional] [default to 50]

### Return type

[**ProjectSecretList**](ProjectSecretList.md)

### Authorization

[operatorBearer](../README.md#operatorBearer)

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Page of Secret Store inventory entries. |  -  |
**400** | Invalid query parameters — typically a tampered or malformed cursor (&#x60;code: invalid_cursor&#x60;) or an out-of-range &#x60;limit&#x60; (&#x60;code: invalid_limit&#x60;).  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to read the secret inventory in the owning Project. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | The owning Project does not exist or is not visible. Body is a &#x60;Problem&#x60; with &#x60;code: project_not_found&#x60;.  |  -  |
**501** | The secret-inventory surface is not yet provisioned in this build. The Problem body&#39;s &#x60;code&#x60; field is &#x60;secrets_inventory_not_provisioned&#x60; so log scrapers can alert on the deferred-wiring state. Resolves once the inventory read service and its authz / audit ports are wired into the v1 handler dependency cone.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

