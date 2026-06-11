# plexsphere.ProvisioningCredentialsApi

All URIs are relative to *http://localhost*

Method | HTTP request | Description
------------- | ------------- | -------------
[**get_credential**](ProvisioningCredentialsApi.md#get_credential) | **GET** /v1/credentials/{id} | Fetch a credential&#39;s lifecycle metadata.
[**list_project_credentials**](ProvisioningCredentialsApi.md#list_project_credentials) | **GET** /v1/projects/{id}/credentials | List the OpenBao credentials owned by a Project.
[**revoke_credential**](ProvisioningCredentialsApi.md#revoke_credential) | **POST** /v1/credentials/{id}/revoke | Revoke a credential.
[**rotate_credential**](ProvisioningCredentialsApi.md#rotate_credential) | **POST** /v1/credentials/{id}/rotate | Rotate a credential&#39;s underlying secret.


# **get_credential**
> CredentialResponse get_credential(id)

Fetch a credential's lifecycle metadata.

Returns the lifecycle metadata for the credential identified by
`{id}`. The credential id does not encode its owning Project,
so the handler must read the row to learn which Project to
authorise against; the ReBAC `observe` check runs on the
resolved parent Project and a denial returns `403` via the
audit-first permission-denied path. A missing row surfaces as
`404 credential_not_found`.

The projection is metadata-only and NEVER exposes the KV mount,
KV path, KV version, or any secret material.


### Example


```python
import plexsphere
from plexsphere.models.credential_response import CredentialResponse
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
    api_instance = plexsphere.ProvisioningCredentialsApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Credential identifier (UUIDv7). Bound on `/v1/credentials/{id}`, `/v1/credentials/{id}/revoke`, and `/v1/credentials/{id}/rotate` for the operator-facing OpenBao Credential Broker read + lifecycle surface. 

    try:
        # Fetch a credential's lifecycle metadata.
        api_response = api_instance.get_credential(id)
        print("The response of ProvisioningCredentialsApi->get_credential:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling ProvisioningCredentialsApi->get_credential: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Credential identifier (UUIDv7). Bound on &#x60;/v1/credentials/{id}&#x60;, &#x60;/v1/credentials/{id}/revoke&#x60;, and &#x60;/v1/credentials/{id}/rotate&#x60; for the operator-facing OpenBao Credential Broker read + lifecycle surface.  | 

### Return type

[**CredentialResponse**](CredentialResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Credential found. |  -  |
**400** | Malformed credential id. Body is a &#x60;Problem&#x60; with &#x60;code: invalid_credential_id&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to observe the parent Project. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Credential not found. Body is a &#x60;Problem&#x60; with &#x60;code: credential_not_found&#x60;.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **list_project_credentials**
> CredentialList list_project_credentials(id, cursor=cursor, limit=limit)

List the OpenBao credentials owned by a Project.

Returns a creation-ordered page of credential lifecycle
metadata for the Project identified by `{id}`. The handler runs
a top-level `observe` ReBAC check on the parent Project BEFORE
the persistence read, then layers a per-row `observe` filter on
top so the response items are the subset of the persistence-
level page the caller is authorised to see.

The projection is metadata-only: it carries the credential
identity, owning Project, version, lifecycle timestamps, and a
derived status. It NEVER exposes the KV mount, KV path, KV
version, or any secret material — the storage location is
deliberately omitted as a storage-internal detail.

The pagination cursor is HMAC-signed and bound to the
per-(caller, pepper) pseudonym, so a cursor minted by one
principal cannot be replayed by another — the cross-caller
replay surfaces as `403 cursor_binding_mismatch`. A tampered
envelope or unknown version byte stays on `400 invalid_cursor`.


### Example


```python
import plexsphere
from plexsphere.models.credential_list import CredentialList
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
    api_instance = plexsphere.ProvisioningCredentialsApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Project identifier (UUIDv7). Bound on `/v1/projects/{id}` for the tenancy CRUD surface and on `/v1/projects/{id}/credentials` for the operator-facing OpenBao Credential Broker inventory list. 
    cursor = 'cursor_example' # str | Opaque continuation token returned by a previous call's `next_cursor`. The encoding is HMAC-signed by the server so a tampered cursor surfaces as `400`.  (optional)
    limit = 50 # int | Maximum number of items to return in a single page. The handler clamps the value to [1, 200] before forwarding it to the read service.  (optional) (default to 50)

    try:
        # List the OpenBao credentials owned by a Project.
        api_response = api_instance.list_project_credentials(id, cursor=cursor, limit=limit)
        print("The response of ProvisioningCredentialsApi->list_project_credentials:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling ProvisioningCredentialsApi->list_project_credentials: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Project identifier (UUIDv7). Bound on &#x60;/v1/projects/{id}&#x60; for the tenancy CRUD surface and on &#x60;/v1/projects/{id}/credentials&#x60; for the operator-facing OpenBao Credential Broker inventory list.  | 
 **cursor** | **str**| Opaque continuation token returned by a previous call&#39;s &#x60;next_cursor&#x60;. The encoding is HMAC-signed by the server so a tampered cursor surfaces as &#x60;400&#x60;.  | [optional] 
 **limit** | **int**| Maximum number of items to return in a single page. The handler clamps the value to [1, 200] before forwarding it to the read service.  | [optional] [default to 50]

### Return type

[**CredentialList**](CredentialList.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Page of credentials. |  -  |
**400** | Invalid query parameters — typically a tampered or malformed cursor, an out-of-range &#x60;limit&#x60;, or a malformed Project id.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to observe the parent Project (body is a &#x60;PermissionDenied&#x60; problem) OR the pagination cursor was minted by a different caller and the per-(caller, pepper) HMAC binding rejected the replay (body is a &#x60;Problem&#x60; with &#x60;code &#x3D; cursor_binding_mismatch&#x60;).  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **revoke_credential**
> CredentialResponse revoke_credential(id, credential_revoke_request)

Revoke a credential.

Revokes the credential identified by `{id}`. The handler reads
the row to resolve the parent Project, runs the `manage` ReBAC
check on that Project, then delegates to the Credential Broker
Custodian which stamps `revoked_at` and appends a
`CredentialRevoked` outbox event in a single transaction.

Revocation is idempotent: revoking an already-revoked
credential returns `200` with the unchanged metadata rather
than an error. The response carries the metadata-only
projection showing the populated `revoked_at`.


### Example


```python
import plexsphere
from plexsphere.models.credential_response import CredentialResponse
from plexsphere.models.credential_revoke_request import CredentialRevokeRequest
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
    api_instance = plexsphere.ProvisioningCredentialsApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Credential identifier (UUIDv7). Bound on `/v1/credentials/{id}`, `/v1/credentials/{id}/revoke`, and `/v1/credentials/{id}/rotate` for the operator-facing OpenBao Credential Broker read + lifecycle surface. 
    credential_revoke_request = {"reason":"rotated out of band by the platform on-call"} # CredentialRevokeRequest | 

    try:
        # Revoke a credential.
        api_response = api_instance.revoke_credential(id, credential_revoke_request)
        print("The response of ProvisioningCredentialsApi->revoke_credential:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling ProvisioningCredentialsApi->revoke_credential: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Credential identifier (UUIDv7). Bound on &#x60;/v1/credentials/{id}&#x60;, &#x60;/v1/credentials/{id}/revoke&#x60;, and &#x60;/v1/credentials/{id}/rotate&#x60; for the operator-facing OpenBao Credential Broker read + lifecycle surface.  | 
 **credential_revoke_request** | [**CredentialRevokeRequest**](CredentialRevokeRequest.md)|  | 

### Return type

[**CredentialResponse**](CredentialResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Credential revoked (or already revoked — the call is idempotent). Body is the metadata-only projection with &#x60;revoked_at&#x60; populated.  |  -  |
**400** | Malformed id or body. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;invalid_credential_id&#x60;, &#x60;invalid_body&#x60;, &#x60;invalid_revoke_reason&#x60; }.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to manage the parent Project. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Credential not found. Body is a &#x60;Problem&#x60; with &#x60;code: credential_not_found&#x60;.  |  -  |
**413** | Request body exceeded the 8 KiB credentials ceiling.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **rotate_credential**
> CredentialResponse rotate_credential(id, credential_rotate_request)

Rotate a credential's underlying secret.

Rotates the credential identified by `{id}`. The handler reads
the row to resolve the parent Project, runs the `manage` ReBAC
check on that Project, then delegates to the Credential Broker
Custodian which writes the new secret material into OpenBao
KV-v2 under a compare-and-swap guard, bumps the broker-row
version, and appends a `CredentialRotated` outbox event in a
single transaction.

The caller supplies `expected_version` — the broker-row version
it observed via a prior read. A mismatch surfaces as
`409 credential_cas_conflict` so two concurrent rotations fail
closed. Rotating a revoked credential is rejected with
`409 credential_revoked`.

Secret material is accepted INBOUND in the request body but is
NEVER returned: the response is the same metadata-only
projection every other read surface returns, with the bumped
`version` and refreshed `expires_at`.


### Example


```python
import plexsphere
from plexsphere.models.credential_response import CredentialResponse
from plexsphere.models.credential_rotate_request import CredentialRotateRequest
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
    api_instance = plexsphere.ProvisioningCredentialsApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Credential identifier (UUIDv7). Bound on `/v1/credentials/{id}`, `/v1/credentials/{id}/revoke`, and `/v1/credentials/{id}/rotate` for the operator-facing OpenBao Credential Broker read + lifecycle surface. 
    credential_rotate_request = {"expected_version":1,"material":{"payload":"c3VwZXItc2VjcmV0LXRva2Vu","ttl_seconds":7776000,"key_values":{"username":"svc-acme"}}} # CredentialRotateRequest | 

    try:
        # Rotate a credential's underlying secret.
        api_response = api_instance.rotate_credential(id, credential_rotate_request)
        print("The response of ProvisioningCredentialsApi->rotate_credential:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling ProvisioningCredentialsApi->rotate_credential: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Credential identifier (UUIDv7). Bound on &#x60;/v1/credentials/{id}&#x60;, &#x60;/v1/credentials/{id}/revoke&#x60;, and &#x60;/v1/credentials/{id}/rotate&#x60; for the operator-facing OpenBao Credential Broker read + lifecycle surface.  | 
 **credential_rotate_request** | [**CredentialRotateRequest**](CredentialRotateRequest.md)|  | 

### Return type

[**CredentialResponse**](CredentialResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Credential rotated. Body is the metadata-only projection with the bumped &#x60;version&#x60; and refreshed &#x60;expires_at&#x60;.  |  -  |
**400** | Malformed id or body. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;invalid_credential_id&#x60;, &#x60;invalid_body&#x60;, &#x60;invalid_rotate_material&#x60; }.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to manage the parent Project. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Credential not found. Body is a &#x60;Problem&#x60; with &#x60;code: credential_not_found&#x60;.  |  -  |
**409** | Optimistic-concurrency or lifecycle conflict. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;credential_cas_conflict&#x60;, &#x60;credential_revoked&#x60; }.  |  -  |
**413** | Request body exceeded the 8 KiB credentials ceiling.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

