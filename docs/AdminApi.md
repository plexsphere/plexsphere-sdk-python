# plexsphere.AdminApi

All URIs are relative to *http://localhost*

Method | HTTP request | Description
------------- | ------------- | -------------
[**delete_admin_group_by_id**](AdminApi.md#delete_admin_group_by_id) | **DELETE** /v1/admin/groups/{id} | Delete a Group.
[**delete_admin_group_member**](AdminApi.md#delete_admin_group_member) | **DELETE** /v1/admin/groups/{id}/members/{principal_id} | Remove a principal from a Group.
[**delete_admin_id_pby_id**](AdminApi.md#delete_admin_id_pby_id) | **DELETE** /v1/admin/idp/{id} | Delete an IdP binding.
[**delete_admin_token_by_id**](AdminApi.md#delete_admin_token_by_id) | **DELETE** /v1/admin/tokens/{id} | Revoke any API token (admin).
[**get_admin_group_by_id**](AdminApi.md#get_admin_group_by_id) | **GET** /v1/admin/groups/{id} | Fetch a Group by identifier.
[**get_admin_group_list**](AdminApi.md#get_admin_group_list) | **GET** /v1/admin/groups | List Groups within a Domain.
[**get_admin_group_members**](AdminApi.md#get_admin_group_members) | **GET** /v1/admin/groups/{id}/members | List members of a Group.
[**get_admin_id_p_list**](AdminApi.md#get_admin_id_p_list) | **GET** /v1/admin/idp | List IdP bindings (optionally filtered by Domain).
[**get_admin_id_pby_id**](AdminApi.md#get_admin_id_pby_id) | **GET** /v1/admin/idp/{id} | Read an IdP binding by identifier.
[**get_admin_tokens**](AdminApi.md#get_admin_tokens) | **GET** /v1/admin/tokens | List API tokens for any owner (admin).
[**patch_admin_group**](AdminApi.md#patch_admin_group) | **PATCH** /v1/admin/groups/{id} | Update mutable fields on a Group.
[**patch_admin_id_p**](AdminApi.md#patch_admin_id_p) | **PATCH** /v1/admin/idp/{id} | Partially update an IdP binding.
[**patch_admin_id_p_status**](AdminApi.md#patch_admin_id_p_status) | **PATCH** /v1/admin/idp/{id}/status | Activate or deactivate an IdP binding.
[**post_admin_group**](AdminApi.md#post_admin_group) | **POST** /v1/admin/groups | Create a Group within a Domain.
[**post_admin_group_member**](AdminApi.md#post_admin_group_member) | **POST** /v1/admin/groups/{id}/members | Add a principal to a Group.
[**post_admin_id_p**](AdminApi.md#post_admin_id_p) | **POST** /v1/admin/idp | Create an IdP binding for a Domain.
[**post_admin_token_rotate**](AdminApi.md#post_admin_token_rotate) | **POST** /v1/admin/tokens/{id}/rotate | Rotate any API token (admin).
[**post_admin_tokens**](AdminApi.md#post_admin_tokens) | **POST** /v1/admin/tokens | Issue an API token on behalf of any principal (admin).


# **delete_admin_group_by_id**
> delete_admin_group_by_id(id)

Delete a Group.

Deletes the Group identified by `{id}` and cascades the
membership rows that reference it.


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
    api_instance = plexsphere.AdminApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Group identifier (UUIDv7).

    try:
        # Delete a Group.
        api_instance.delete_admin_group_by_id(id)
    except Exception as e:
        print("Exception when calling AdminApi->delete_admin_group_by_id: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Group identifier (UUIDv7). | 

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
**204** | Group deleted. |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to delete the Group. Body is a &#x60;PermissionDenied&#x60; problem carrying the ReBAC denial &#x60;reason&#x60;, traversed &#x60;relation_path&#x60;, and the &#x60;correlation_id&#x60; that pairs with the audit entry .  |  -  |
**404** | Group not found. |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **delete_admin_group_member**
> delete_admin_group_member(id, principal_id, kind)

Remove a principal from a Group.

Removes the Membership identified by `(id, principal_id, kind)`
from the Group.


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
    api_instance = plexsphere.AdminApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Group identifier (UUIDv7).
    principal_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Principal identifier (UUIDv7).
    kind = 'kind_example' # str | Principal discriminator. DECISION: `kind` is REQUIRED because the underlying `group_memberships` table uses XOR columns (`principal_user_id` / `principal_service_identity_id` / `principal_group_id`) and the repo needs the discriminator to target the correct column — there is no `principal_id`-only index. 

    try:
        # Remove a principal from a Group.
        api_instance.delete_admin_group_member(id, principal_id, kind)
    except Exception as e:
        print("Exception when calling AdminApi->delete_admin_group_member: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Group identifier (UUIDv7). | 
 **principal_id** | **UUID**| Principal identifier (UUIDv7). | 
 **kind** | **str**| Principal discriminator. DECISION: &#x60;kind&#x60; is REQUIRED because the underlying &#x60;group_memberships&#x60; table uses XOR columns (&#x60;principal_user_id&#x60; / &#x60;principal_service_identity_id&#x60; / &#x60;principal_group_id&#x60;) and the repo needs the discriminator to target the correct column — there is no &#x60;principal_id&#x60;-only index.  | 

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
**204** | Membership deleted. |  -  |
**400** | Invalid &#x60;kind&#x60; value. |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to remove the Group member. Body is a &#x60;PermissionDenied&#x60; problem carrying the ReBAC denial &#x60;reason&#x60;, traversed &#x60;relation_path&#x60;, and the &#x60;correlation_id&#x60; that pairs with the audit entry .  |  -  |
**404** | Group or Membership not found. |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **delete_admin_id_pby_id**
> delete_admin_id_pby_id(id)

Delete an IdP binding.

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
    api_instance = plexsphere.AdminApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Binding identifier (UUIDv7).

    try:
        # Delete an IdP binding.
        api_instance.delete_admin_id_pby_id(id)
    except Exception as e:
        print("Exception when calling AdminApi->delete_admin_id_pby_id: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Binding identifier (UUIDv7). | 

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
**204** | Binding deleted. |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to delete the IdP binding. Body is a &#x60;PermissionDenied&#x60; problem carrying the ReBAC denial &#x60;reason&#x60;, traversed &#x60;relation_path&#x60;, and the &#x60;correlation_id&#x60; that pairs with the audit entry .  |  -  |
**404** | Binding not found. |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **delete_admin_token_by_id**
> delete_admin_token_by_id(id)

Revoke any API token (admin).

Immediately revokes the token identified by `id`, regardless of
ownership. Gated on the `manage` relation against the owning
principal's Domain; rejection surfaces as `PermissionDenied`.


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
    api_instance = plexsphere.AdminApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Token identifier (UUIDv7).

    try:
        # Revoke any API token (admin).
        api_instance.delete_admin_token_by_id(id)
    except Exception as e:
        print("Exception when calling AdminApi->delete_admin_token_by_id: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Token identifier (UUIDv7). | 

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
**204** | Token revoked. |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to revoke the token. Body is a &#x60;PermissionDenied&#x60; problem carrying the ReBAC denial &#x60;reason&#x60;, traversed &#x60;relation_path&#x60;, and the &#x60;correlation_id&#x60;.  |  -  |
**404** | Token not found. |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **get_admin_group_by_id**
> GroupResponse get_admin_group_by_id(id)

Fetch a Group by identifier.

Returns the Group aggregate identified by `{id}`
.


### Example


```python
import plexsphere
from plexsphere.models.group_response import GroupResponse
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
    api_instance = plexsphere.AdminApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Group identifier (UUIDv7).

    try:
        # Fetch a Group by identifier.
        api_response = api_instance.get_admin_group_by_id(id)
        print("The response of AdminApi->get_admin_group_by_id:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling AdminApi->get_admin_group_by_id: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Group identifier (UUIDv7). | 

### Return type

[**GroupResponse**](GroupResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Group found. |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to read the Group. Body is a &#x60;PermissionDenied&#x60; problem carrying the ReBAC denial &#x60;reason&#x60;, traversed &#x60;relation_path&#x60;, and the &#x60;correlation_id&#x60; that pairs with the audit entry.  |  -  |
**404** | Group not found. |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **get_admin_group_list**
> GroupListResponse get_admin_group_list(domain_id, cursor=cursor, limit=limit)

List Groups within a Domain.

Returns Groups for the supplied Domain in deterministic order
with cursor pagination.


### Example


```python
import plexsphere
from plexsphere.models.group_list_response import GroupListResponse
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
    api_instance = plexsphere.AdminApi(api_client)
    domain_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Domain. DECISION: `domain_id` is REQUIRED here (unlike the IdP list endpoint where it is optional) because Group slugs are scoped per-Domain and listing across domains would require cross-domain cursor merging that the repo layer does not support and is not needed by the admin UI. 
    cursor = 'cursor_example' # str | Opaque continuation token returned by a previous call's `next_cursor`.  (optional)
    limit = 50 # int | Maximum number of items to return in a single page .  (optional) (default to 50)

    try:
        # List Groups within a Domain.
        api_response = api_instance.get_admin_group_list(domain_id, cursor=cursor, limit=limit)
        print("The response of AdminApi->get_admin_group_list:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling AdminApi->get_admin_group_list: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **domain_id** | **UUID**| Owning Domain. DECISION: &#x60;domain_id&#x60; is REQUIRED here (unlike the IdP list endpoint where it is optional) because Group slugs are scoped per-Domain and listing across domains would require cross-domain cursor merging that the repo layer does not support and is not needed by the admin UI.  | 
 **cursor** | **str**| Opaque continuation token returned by a previous call&#39;s &#x60;next_cursor&#x60;.  | [optional] 
 **limit** | **int**| Maximum number of items to return in a single page .  | [optional] [default to 50]

### Return type

[**GroupListResponse**](GroupListResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Page of Groups. |  -  |
**400** | Invalid query parameters. |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to read Groups in the addressed Domain. Body is a &#x60;PermissionDenied&#x60; problem carrying the ReBAC denial &#x60;reason&#x60;, traversed &#x60;relation_path&#x60;, and the &#x60;correlation_id&#x60; that pairs with the audit entry.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **get_admin_group_members**
> GroupMembershipListResponse get_admin_group_members(id)

List members of a Group.

Returns the Membership rows attached to the Group identified
by `{id}`.


### Example


```python
import plexsphere
from plexsphere.models.group_membership_list_response import GroupMembershipListResponse
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
    api_instance = plexsphere.AdminApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Group identifier (UUIDv7).

    try:
        # List members of a Group.
        api_response = api_instance.get_admin_group_members(id)
        print("The response of AdminApi->get_admin_group_members:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling AdminApi->get_admin_group_members: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Group identifier (UUIDv7). | 

### Return type

[**GroupMembershipListResponse**](GroupMembershipListResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Memberships of the Group. |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to list members of the Group. Body is a &#x60;PermissionDenied&#x60; problem carrying the ReBAC denial &#x60;reason&#x60;, traversed &#x60;relation_path&#x60;, and the &#x60;correlation_id&#x60; that pairs with the audit entry.  |  -  |
**404** | Group not found. |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **get_admin_id_p_list**
> List[IdPBindingResponse] get_admin_id_p_list(domain_id=domain_id)

List IdP bindings (optionally filtered by Domain).

### Example


```python
import plexsphere
from plexsphere.models.id_p_binding_response import IdPBindingResponse
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
    api_instance = plexsphere.AdminApi(api_client)
    domain_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Optional Domain filter. (optional)

    try:
        # List IdP bindings (optionally filtered by Domain).
        api_response = api_instance.get_admin_id_p_list(domain_id=domain_id)
        print("The response of AdminApi->get_admin_id_p_list:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling AdminApi->get_admin_id_p_list: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **domain_id** | **UUID**| Optional Domain filter. | [optional] 

### Return type

[**List[IdPBindingResponse]**](IdPBindingResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | List of IdP bindings. |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to read IdP bindings in the addressed Domain. Body is a &#x60;PermissionDenied&#x60; problem carrying the ReBAC denial &#x60;reason&#x60;, traversed &#x60;relation_path&#x60;, and the &#x60;correlation_id&#x60; that pairs with the audit entry.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **get_admin_id_pby_id**
> IdPBindingResponse get_admin_id_pby_id(id)

Read an IdP binding by identifier.

Returns the persisted IdP binding aggregate identified by `id`
. A 404 is returned with `binding-not-found`
semantics when no binding with the given identifier exists or
the caller may not observe it.


### Example


```python
import plexsphere
from plexsphere.models.id_p_binding_response import IdPBindingResponse
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
    api_instance = plexsphere.AdminApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Binding identifier (UUIDv7).

    try:
        # Read an IdP binding by identifier.
        api_response = api_instance.get_admin_id_pby_id(id)
        print("The response of AdminApi->get_admin_id_pby_id:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling AdminApi->get_admin_id_pby_id: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Binding identifier (UUIDv7). | 

### Return type

[**IdPBindingResponse**](IdPBindingResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Binding found. |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to read the IdP binding. Body is a &#x60;PermissionDenied&#x60; problem carrying the ReBAC denial &#x60;reason&#x60;, traversed &#x60;relation_path&#x60;, and the &#x60;correlation_id&#x60; that pairs with the audit entry .  |  -  |
**404** | Binding not found (&#x60;binding-not-found&#x60;). |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **get_admin_tokens**
> List[APITokenSummary] get_admin_tokens(identity_ref)

List API tokens for any owner (admin).

Returns API-token summaries for the principal addressed by
`identity_ref` (e.g. `user:<uuid>` or `service:<uuid>`).
Plaintext is never included. Gated on the `manage` relation
against the owner's Domain so the same authz check governs
both reads and mutations.


### Example


```python
import plexsphere
from plexsphere.models.api_token_summary import APITokenSummary
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
    api_instance = plexsphere.AdminApi(api_client)
    identity_ref = 'identity_ref_example' # str | Owner reference in the canonical `user:<uuid>` or `service:<uuid>` form mirroring `APITokenIssueRequest`. 

    try:
        # List API tokens for any owner (admin).
        api_response = api_instance.get_admin_tokens(identity_ref)
        print("The response of AdminApi->get_admin_tokens:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling AdminApi->get_admin_tokens: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **identity_ref** | **str**| Owner reference in the canonical &#x60;user:&lt;uuid&gt;&#x60; or &#x60;service:&lt;uuid&gt;&#x60; form mirroring &#x60;APITokenIssueRequest&#x60;.  | 

### Return type

[**List[APITokenSummary]**](APITokenSummary.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | List of token summaries for the addressed owner. |  -  |
**400** | Invalid &#x60;identity_ref&#x60; (missing, wrong shape, bad UUID). |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to read tokens in the addressed owner&#39;s Domain. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **patch_admin_group**
> GroupResponse patch_admin_group(id, group_update_request)

Update mutable fields on a Group.

Updates the Group identified by `{id}`. Only `display_name` is
settable; the `slug` is immutable because it participates in
the `(domain_id, slug)` uniqueness constraint and is referenced
by IdP claim mappings.


### Example


```python
import plexsphere
from plexsphere.models.group_response import GroupResponse
from plexsphere.models.group_update_request import GroupUpdateRequest
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
    api_instance = plexsphere.AdminApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Group identifier (UUIDv7).
    group_update_request = plexsphere.GroupUpdateRequest() # GroupUpdateRequest | 

    try:
        # Update mutable fields on a Group.
        api_response = api_instance.patch_admin_group(id, group_update_request)
        print("The response of AdminApi->patch_admin_group:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling AdminApi->patch_admin_group: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Group identifier (UUIDv7). | 
 **group_update_request** | [**GroupUpdateRequest**](GroupUpdateRequest.md)|  | 

### Return type

[**GroupResponse**](GroupResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Group updated. |  -  |
**400** | Invalid update body. |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to update the Group. Body is a &#x60;PermissionDenied&#x60; problem carrying the ReBAC denial &#x60;reason&#x60;, traversed &#x60;relation_path&#x60;, and the &#x60;correlation_id&#x60; that pairs with the audit entry .  |  -  |
**404** | Group not found. |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **patch_admin_id_p**
> IdPBindingResponse patch_admin_id_p(id, id_p_binding_patch_request)

Partially update an IdP binding.

Applies a partial update to the IdP binding aggregate identified
by `id`. Only the fields present in
the request body are mutated; omitted fields are left unchanged.
The `status` field is intentionally NOT mutable through this
endpoint — use `PATCH /v1/admin/idp/{id}/status` instead. The
`domain_id` and `issuer` columns are NOT exposed by
`IdPBindingPatchRequest`, so this operation cannot collide with
the partial unique index `idp_bindings_active_domain_issuer_uq`.

Concurrent PATCH callers are serialised via an optimistic-
concurrency compare-and-swap on the binding's `version`
column: each PATCH transaction reads the row under
`SELECT... FOR UPDATE`, captures the version, and gates its
UPDATE on `version = expected_version`. Two concurrent PATCHes
therefore race deterministically — exactly ONE wins (response
`200` with the new aggregate) and the loser receives a
`409 binding-conflict` Problem so the client can refresh and
retry against the post-winner state (,; review
#1, comment 2).


### Example


```python
import plexsphere
from plexsphere.models.id_p_binding_patch_request import IdPBindingPatchRequest
from plexsphere.models.id_p_binding_response import IdPBindingResponse
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
    api_instance = plexsphere.AdminApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Binding identifier (UUIDv7).
    id_p_binding_patch_request = plexsphere.IdPBindingPatchRequest() # IdPBindingPatchRequest | 

    try:
        # Partially update an IdP binding.
        api_response = api_instance.patch_admin_id_p(id, id_p_binding_patch_request)
        print("The response of AdminApi->patch_admin_id_p:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling AdminApi->patch_admin_id_p: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Binding identifier (UUIDv7). | 
 **id_p_binding_patch_request** | [**IdPBindingPatchRequest**](IdPBindingPatchRequest.md)|  | 

### Return type

[**IdPBindingResponse**](IdPBindingResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Binding updated. |  -  |
**400** | Invalid patch body — for example malformed JSON, an empty patch, or an unsupported &#x60;jit_policy&#x60; value.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to update the IdP binding. Body is a &#x60;PermissionDenied&#x60; problem carrying the ReBAC denial &#x60;reason&#x60;, traversed &#x60;relation_path&#x60;, and the &#x60;correlation_id&#x60; that pairs with the audit entry .  |  -  |
**404** | Binding not found (&#x60;binding-not-found&#x60;). |  -  |
**409** | Optimistic-concurrency conflict (&#x60;binding-conflict&#x60;). Two PATCH callers raced on the same binding: one committed first and bumped the row&#39;s &#x60;version&#x60;; this caller&#39;s UPDATE saw a stale expected version and matched zero rows. The client should re-read the binding (GET /v1/admin/idp/{id}) and re-apply the patch on top of the fresh aggregate.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **patch_admin_id_p_status**
> IdPBindingResponse patch_admin_id_p_status(id, id_p_binding_status_request)

Activate or deactivate an IdP binding.

Toggles the `status` of the named binding between `active` and
`deactivated`. The response echoes the full updated binding
.


### Example


```python
import plexsphere
from plexsphere.models.id_p_binding_response import IdPBindingResponse
from plexsphere.models.id_p_binding_status_request import IdPBindingStatusRequest
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
    api_instance = plexsphere.AdminApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Binding identifier (UUIDv7).
    id_p_binding_status_request = plexsphere.IdPBindingStatusRequest() # IdPBindingStatusRequest | 

    try:
        # Activate or deactivate an IdP binding.
        api_response = api_instance.patch_admin_id_p_status(id, id_p_binding_status_request)
        print("The response of AdminApi->patch_admin_id_p_status:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling AdminApi->patch_admin_id_p_status: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Binding identifier (UUIDv7). | 
 **id_p_binding_status_request** | [**IdPBindingStatusRequest**](IdPBindingStatusRequest.md)|  | 

### Return type

[**IdPBindingResponse**](IdPBindingResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Status updated. |  -  |
**400** | Unsupported status value. |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to toggle the IdP binding status. Body is a &#x60;PermissionDenied&#x60; problem carrying the ReBAC denial &#x60;reason&#x60;, traversed &#x60;relation_path&#x60;, and the &#x60;correlation_id&#x60; that pairs with the audit entry .  |  -  |
**404** | Binding not found. |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **post_admin_group**
> GroupResponse post_admin_group(group_request)

Create a Group within a Domain.

Creates a Group aggregate scoped to the supplied Domain
. The aggregate enforces the source-specific
invariant: `source=idp` requires both `idp_binding_id` and
`idp_claim_value`; `source=manual` forbids them. Violations
surface as a 400 Problem from validation, not as a SQL CHECK
failure.


### Example


```python
import plexsphere
from plexsphere.models.group_request import GroupRequest
from plexsphere.models.group_response import GroupResponse
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
    api_instance = plexsphere.AdminApi(api_client)
    group_request = plexsphere.GroupRequest() # GroupRequest | 

    try:
        # Create a Group within a Domain.
        api_response = api_instance.post_admin_group(group_request)
        print("The response of AdminApi->post_admin_group:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling AdminApi->post_admin_group: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **group_request** | [**GroupRequest**](GroupRequest.md)|  | 

### Return type

[**GroupResponse**](GroupResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**201** | Group created. |  -  |
**400** | Invalid Group body or source/idp invariant violated. |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to manage Groups. Body is a &#x60;PermissionDenied&#x60; problem carrying the ReBAC denial &#x60;reason&#x60;, traversed &#x60;relation_path&#x60;, and the &#x60;correlation_id&#x60; that pairs with the audit entry .  |  -  |
**409** | Conflict — another Group with the same &#x60;(domain_id, slug)&#x60; pair already exists.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **post_admin_group_member**
> GroupMembershipResponse post_admin_group_member(id, group_membership_request)

Add a principal to a Group.

Adds the supplied principal to the Group identified by `{id}`.
The Membership's `source` MUST match the parent Group's
`source`; a mismatch returns 409 source-conflict. The per-Group
claim mapping lives on the parent Group aggregate, not on each
Membership row — the request body therefore does NOT carry
`idp_claim_value`.


### Example


```python
import plexsphere
from plexsphere.models.group_membership_request import GroupMembershipRequest
from plexsphere.models.group_membership_response import GroupMembershipResponse
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
    api_instance = plexsphere.AdminApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Group identifier (UUIDv7).
    group_membership_request = plexsphere.GroupMembershipRequest() # GroupMembershipRequest | 

    try:
        # Add a principal to a Group.
        api_response = api_instance.post_admin_group_member(id, group_membership_request)
        print("The response of AdminApi->post_admin_group_member:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling AdminApi->post_admin_group_member: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Group identifier (UUIDv7). | 
 **group_membership_request** | [**GroupMembershipRequest**](GroupMembershipRequest.md)|  | 

### Return type

[**GroupMembershipResponse**](GroupMembershipResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**201** | Membership created. |  -  |
**400** | Invalid membership body or source/idp invariant violated. |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to add Group members. Body is a &#x60;PermissionDenied&#x60; problem carrying the ReBAC denial &#x60;reason&#x60;, traversed &#x60;relation_path&#x60;, and the &#x60;correlation_id&#x60; that pairs with the audit entry .  |  -  |
**404** | Group not found. |  -  |
**409** | Conflict — a Membership with the same &#x60;(group_id, principal_kind, principal_id)&#x60; triple already exists .  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **post_admin_id_p**
> IdPBindingResponse post_admin_id_p(id_p_binding_request)

Create an IdP binding for a Domain.

Creates a new IdP binding. The response
echoes the persisted aggregate but never includes the client
secret itself — only the opaque `client_secret_ref`.


### Example


```python
import plexsphere
from plexsphere.models.id_p_binding_request import IdPBindingRequest
from plexsphere.models.id_p_binding_response import IdPBindingResponse
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
    api_instance = plexsphere.AdminApi(api_client)
    id_p_binding_request = plexsphere.IdPBindingRequest() # IdPBindingRequest | 

    try:
        # Create an IdP binding for a Domain.
        api_response = api_instance.post_admin_id_p(id_p_binding_request)
        print("The response of AdminApi->post_admin_id_p:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling AdminApi->post_admin_id_p: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id_p_binding_request** | [**IdPBindingRequest**](IdPBindingRequest.md)|  | 

### Return type

[**IdPBindingResponse**](IdPBindingResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**201** | Binding created. |  -  |
**400** | Invalid binding body. |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to manage IdP bindings. Body is a &#x60;PermissionDenied&#x60; RFC 9457 problem carrying the ReBAC denial &#x60;reason&#x60;, traversed &#x60;relation_path&#x60;, and the &#x60;correlation_id&#x60; that pairs the response with the audit entry emitted by &#x60;internal/audit&#x60;.  |  -  |
**409** | Conflict — another active binding exists for the same (domain_id, issuer) pair.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **post_admin_token_rotate**
> APITokenRotateResponse post_admin_token_rotate(id)

Rotate any API token (admin).

Issues a replacement plaintext for the named token regardless
of ownership and retires the previous plaintext with an RFC
8594 `Sunset` header. Gated on the `manage` relation against
the owning principal's Domain.


### Example


```python
import plexsphere
from plexsphere.models.api_token_rotate_response import APITokenRotateResponse
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
    api_instance = plexsphere.AdminApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Token identifier (UUIDv7).

    try:
        # Rotate any API token (admin).
        api_response = api_instance.post_admin_token_rotate(id)
        print("The response of AdminApi->post_admin_token_rotate:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling AdminApi->post_admin_token_rotate: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Token identifier (UUIDv7). | 

### Return type

[**APITokenRotateResponse**](APITokenRotateResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Token rotated; plaintext is returned exactly once. |  * Sunset - RFC 8594 sunset timestamp for the retired plaintext. <br>  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to rotate the token. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Token not found. |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **post_admin_tokens**
> APITokenIssueResponse post_admin_tokens(api_token_issue_request)

Issue an API token on behalf of any principal (admin).

Admin-scope issuance of a psk-shaped API token bound to the
supplied `identity_ref` — the caller does NOT need to own the
target identity. Gated on the `manage` relation against the
target principal's owning Domain; rejection surfaces as
`PermissionDenied`. Mirrors `PostAuthTokens` but bypasses the
self-only check; plaintext is returned exactly once.


### Example


```python
import plexsphere
from plexsphere.models.api_token_issue_request import APITokenIssueRequest
from plexsphere.models.api_token_issue_response import APITokenIssueResponse
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
    api_instance = plexsphere.AdminApi(api_client)
    api_token_issue_request = plexsphere.APITokenIssueRequest() # APITokenIssueRequest | 

    try:
        # Issue an API token on behalf of any principal (admin).
        api_response = api_instance.post_admin_tokens(api_token_issue_request)
        print("The response of AdminApi->post_admin_tokens:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling AdminApi->post_admin_tokens: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **api_token_issue_request** | [**APITokenIssueRequest**](APITokenIssueRequest.md)|  | 

### Return type

[**APITokenIssueResponse**](APITokenIssueResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**201** | Token issued; plaintext is included exactly once. |  -  |
**400** | Invalid body (bad env_prefix, missing identity_ref). |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to manage tokens in the target principal&#39;s Domain. Body is a &#x60;PermissionDenied&#x60; RFC 9457 problem carrying the ReBAC denial &#x60;reason&#x60;, traversed &#x60;relation_path&#x60;, and the &#x60;correlation_id&#x60; that pairs the response with the audit entry emitted by &#x60;internal/audit&#x60;.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

