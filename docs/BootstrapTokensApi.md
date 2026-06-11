# plexsphere.BootstrapTokensApi

All URIs are relative to *http://localhost*

Method | HTTP request | Description
------------- | ------------- | -------------
[**get_bootstrap_token_metadata**](BootstrapTokensApi.md#get_bootstrap_token_metadata) | **GET** /v1/projects/{project_id}/bootstrap-tokens/{id} | Fetch BootstrapToken metadata by identifier.
[**issue_bootstrap_token**](BootstrapTokensApi.md#issue_bootstrap_token) | **POST** /v1/projects/{project_id}/bootstrap-tokens | Issue a BootstrapToken for a Project.
[**list_bootstrap_tokens**](BootstrapTokensApi.md#list_bootstrap_tokens) | **GET** /v1/projects/{project_id}/bootstrap-tokens | List BootstrapTokens issued for a Project.
[**post_register**](BootstrapTokensApi.md#post_register) | **POST** /v1/register | Redeem a BootstrapToken to enrol a Node into a Domain.
[**revoke_bootstrap_token**](BootstrapTokensApi.md#revoke_bootstrap_token) | **DELETE** /v1/projects/{project_id}/bootstrap-tokens/{id} | Revoke a BootstrapToken.


# **get_bootstrap_token_metadata**
> BootstrapTokenMetadata get_bootstrap_token_metadata(project_id, id)

Fetch BootstrapToken metadata by identifier.

Returns metadata for the BootstrapToken identified by `{id}`
within the target Project. The plaintext is NEVER returned —
the persistence layer holds only an Argon2id hash, and the
plaintext window closed when the issue response was written
.


### Example


```python
import plexsphere
from plexsphere.models.bootstrap_token_metadata import BootstrapTokenMetadata
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
    api_instance = plexsphere.BootstrapTokensApi(api_client)
    project_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Project. The BootstrapToken aggregate is scoped per Project so every administration endpoint requires the Project identifier in the path. 
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | BootstrapToken identifier (UUIDv7).

    try:
        # Fetch BootstrapToken metadata by identifier.
        api_response = api_instance.get_bootstrap_token_metadata(project_id, id)
        print("The response of BootstrapTokensApi->get_bootstrap_token_metadata:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling BootstrapTokensApi->get_bootstrap_token_metadata: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **project_id** | **UUID**| Owning Project. The BootstrapToken aggregate is scoped per Project so every administration endpoint requires the Project identifier in the path.  | 
 **id** | **UUID**| BootstrapToken identifier (UUIDv7). | 

### Return type

[**BootstrapTokenMetadata**](BootstrapTokenMetadata.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | BootstrapToken metadata. |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to read BootstrapTokens in the target Project. Body is a &#x60;PermissionDenied&#x60; problem .  |  -  |
**404** | BootstrapToken or Project not found. |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **issue_bootstrap_token**
> BootstrapTokenIssueResponse issue_bootstrap_token(project_id, bootstrap_token_issue_request)

Issue a BootstrapToken for a Project.

Mints a fresh BootstrapToken scoped to the supplied Project
. The aggregate enforces the issuance
invariants (`kind` in `{node, bridge}`, `env_prefix` matches
`^[a-z]+$`, TTL inside `[300, 86400]` seconds); violations
surface as a 400 Problem from validation, not as a SQL CHECK
failure.

The response body is the **only** time the plaintext token is
ever exposed. The persistence layer stores an Argon2id hash
only, so once the response has been written the plaintext can
no longer be retrieved through any API surface. Operators MUST capture the plaintext from
this response and hand it to the redeeming Node/Bridge
out-of-band; a lost plaintext requires re-issuing the token.


### Example


```python
import plexsphere
from plexsphere.models.bootstrap_token_issue_request import BootstrapTokenIssueRequest
from plexsphere.models.bootstrap_token_issue_response import BootstrapTokenIssueResponse
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
    api_instance = plexsphere.BootstrapTokensApi(api_client)
    project_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Project. The BootstrapToken aggregate is scoped per Project so every administration endpoint requires the Project identifier in the path. 
    bootstrap_token_issue_request = {"kind":"node","env_prefix":"prod","ttl_seconds":3600} # BootstrapTokenIssueRequest | 

    try:
        # Issue a BootstrapToken for a Project.
        api_response = api_instance.issue_bootstrap_token(project_id, bootstrap_token_issue_request)
        print("The response of BootstrapTokensApi->issue_bootstrap_token:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling BootstrapTokensApi->issue_bootstrap_token: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **project_id** | **UUID**| Owning Project. The BootstrapToken aggregate is scoped per Project so every administration endpoint requires the Project identifier in the path.  | 
 **bootstrap_token_issue_request** | [**BootstrapTokenIssueRequest**](BootstrapTokenIssueRequest.md)|  | 

### Return type

[**BootstrapTokenIssueResponse**](BootstrapTokenIssueResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**201** | BootstrapToken issued. The &#x60;token&#x60; field carries the plaintext exactly once; subsequent reads return only the metadata view.  |  -  |
**400** | Invalid issue body (unknown &#x60;kind&#x60;, malformed &#x60;env_prefix&#x60;, TTL out of range).  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to issue BootstrapTokens in the target Project. Body is a &#x60;PermissionDenied&#x60; problem carrying the ReBAC denial &#x60;reason&#x60;, traversed &#x60;relation_path&#x60;, and &#x60;correlation_id&#x60; that pairs with the audit entry.  |  -  |
**404** | Project not found. |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **list_bootstrap_tokens**
> BootstrapTokenList list_bootstrap_tokens(project_id, cursor=cursor, limit=limit)

List BootstrapTokens issued for a Project.

Returns BootstrapToken metadata for the supplied Project in
deterministic order with cursor pagination. The plaintext is
NEVER included in this response — only the hash-derived
metadata view.


### Example


```python
import plexsphere
from plexsphere.models.bootstrap_token_list import BootstrapTokenList
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
    api_instance = plexsphere.BootstrapTokensApi(api_client)
    project_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Project. The BootstrapToken aggregate is scoped per Project so every administration endpoint requires the Project identifier in the path. 
    cursor = 'cursor_example' # str | Opaque continuation token returned by a previous call's `next_cursor`.  (optional)
    limit = 50 # int | Maximum number of items to return in a single page .  (optional) (default to 50)

    try:
        # List BootstrapTokens issued for a Project.
        api_response = api_instance.list_bootstrap_tokens(project_id, cursor=cursor, limit=limit)
        print("The response of BootstrapTokensApi->list_bootstrap_tokens:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling BootstrapTokensApi->list_bootstrap_tokens: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **project_id** | **UUID**| Owning Project. The BootstrapToken aggregate is scoped per Project so every administration endpoint requires the Project identifier in the path.  | 
 **cursor** | **str**| Opaque continuation token returned by a previous call&#39;s &#x60;next_cursor&#x60;.  | [optional] 
 **limit** | **int**| Maximum number of items to return in a single page .  | [optional] [default to 50]

### Return type

[**BootstrapTokenList**](BootstrapTokenList.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Page of BootstrapToken metadata. |  -  |
**400** | Invalid query parameters. |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to list BootstrapTokens in the target Project. Body is a &#x60;PermissionDenied&#x60; problem .  |  -  |
**404** | Project not found. |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **post_register**
> RegisterResponse post_register(register_request)

Redeem a BootstrapToken to enrol a Node into a Domain.

Consumes a previously-issued BootstrapToken plaintext and
provisions a complete Node identity inside the redeeming
Domain. The handler:

  1. Validates the inbound public key (length 32 after base64
     decode, all-zero rejection) BEFORE any token consumption
     attempt so a malformed key cannot also waste a token
     .
  2. Runs the BootstrapToken plaintext through
     `bootstraptokens.Validator.Consume` for one-shot,
     project-scoped, kind-scoped enforcement.
  3. Allocates a mesh-IP from the Domain's pool — flat by
     default, drawn from the optional Project sub-range when
     one applies.
  4. Issues the per-Node Node Secret Key (NSK) plaintext +
     wrapped persistence form through the configured wrap-key
     adapter.
  5. Persists Node + IP-allocation + NSK + outbox event in
     one pgx transaction; a partial failure rolls back every
     step including the BootstrapToken consume.

The endpoint is the unauthenticated bootstrap seam — the
BootstrapToken plaintext IS the credential the redeeming
substrate presents — so the caller does NOT pass an
Authorization header here. The Spectral
`plexsphere-write-once-post-must-be-issue-response` rule
guards the `nsk` field's `x-plexsphere-once: true` marker so
the plaintext is only ever returned by THIS one operation
.


### Example


```python
import plexsphere
from plexsphere.models.register_request import RegisterRequest
from plexsphere.models.register_response import RegisterResponse
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
    api_instance = plexsphere.BootstrapTokensApi(api_client)
    register_request = {"project_id":"0190a8b8-a0c0-7a0a-8a0a-a0a0a0a0a0a0","resource_id":"edge-router-01","bootstrap_token":"psb_prod_aebagbafaydqqbrhibbsa3kqaq_node_xxxxxxxxxxxxxxxxxxxxxxxxxx","nonce":"f3f8c0b8-7a0a-8a0a-a0a0-a0a0a0a0a0a0","public_key":"AAECAwQFBgcICQoLDA0ODxAREhMUFRYXGBkaGxwdHh8="} # RegisterRequest | 

    try:
        # Redeem a BootstrapToken to enrol a Node into a Domain.
        api_response = api_instance.post_register(register_request)
        print("The response of BootstrapTokensApi->post_register:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling BootstrapTokensApi->post_register: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **register_request** | [**RegisterRequest**](RegisterRequest.md)|  | 

### Return type

[**RegisterResponse**](RegisterResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | BootstrapToken redeemed successfully. The redeeming Node is now bound to the named Project + Resource; the response carries the freshly-allocated mesh-IP, the per-Node NSK plaintext (returned exactly once — see the &#x60;x-plexsphere-once: true&#x60; marker on the &#x60;nsk&#x60; field), the Domain&#39;s signing public key + key id, the initial wireguard peer snapshot, and the Domain&#39;s mesh CIDR .  |  -  |
**400** | Inbound &#x60;public_key&#x60; failed the structural validation gate — wrong byte length after base64 decode, all-zero degenerate value, or unparseable encoding. The BootstrapToken is NOT consumed when this branch fires; the operator can retry with a corrected key against the same plaintext.  |  -  |
**403** | Plaintext BootstrapToken failed the Validator precedence check. The &#x60;code&#x60; field on the &#x60;Problem&#x60; body carries the canonical denial reason: &#x60;kind_mismatch&#x60;, &#x60;project_mismatch&#x60;, &#x60;token_consumed&#x60;, &#x60;token_expired&#x60;, &#x60;token_revoked&#x60;, or &#x60;nonce_collision&#x60;. The schema is &#x60;Problem&#x60; (not &#x60;PermissionDenied&#x60;) because /v1/register is the unauthenticated bootstrap seam — denials surface from BootstrapToken validation, NOT from a ReBAC check .  |  -  |
**404** | Project or Resource named in the RegisterRequest body could not be resolved. The BootstrapToken is NOT consumed when this branch fires — a missing Resource is an operator-actionable misconfiguration, not a replay invitation.  |  -  |
**422** | RegisterRequest body parsed but failed the application- boundary invariant checks (e.g. an empty &#x60;bootstrap_token&#x60;, a zero-valued &#x60;project_id&#x60; UUID, an empty &#x60;resource_id&#x60;). Distinct from 400 (which is reserved for &#x60;public_key&#x60; shape failures) so operators can disambiguate the two cleanly.  |  -  |
**503** | Mesh-IP allocator could not place the Node: either the flat Domain pool or a strict-mode Project sub-range is exhausted, or the per-Domain advisory lock was contended past the configured wait budget. The Problem &#x60;code&#x60; field carries the canonical reason (&#x60;pool_exhausted&#x60;, &#x60;subrange_exhausted&#x60;, &#x60;allocator_contention&#x60;). The BootstrapToken is NOT consumed when this branch fires — pool exhaustion is an operator-actionable capacity event, not a replay invitation.  |  -  |
**500** | Internal server error. NSK issuance failures, outbox append failures, or any other infrastructure-side fault inside the registration tx surface here. The BootstrapToken is NOT consumed because the registration transaction rolls back atomically.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **revoke_bootstrap_token**
> revoke_bootstrap_token(project_id, id)

Revoke a BootstrapToken.

Revokes the BootstrapToken identified by `{id}` so a future
redemption attempt is rejected even if the plaintext is still
in flight. Revocation is a
terminal aggregate transition: a token that is already
consumed or already revoked rejects the call with 409 so the
operator can distinguish "I beat the redeemer" from "the
redeemer beat me".


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
    api_instance = plexsphere.BootstrapTokensApi(api_client)
    project_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Project. The BootstrapToken aggregate is scoped per Project so every administration endpoint requires the Project identifier in the path. 
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | BootstrapToken identifier (UUIDv7).

    try:
        # Revoke a BootstrapToken.
        api_instance.revoke_bootstrap_token(project_id, id)
    except Exception as e:
        print("Exception when calling BootstrapTokensApi->revoke_bootstrap_token: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **project_id** | **UUID**| Owning Project. The BootstrapToken aggregate is scoped per Project so every administration endpoint requires the Project identifier in the path.  | 
 **id** | **UUID**| BootstrapToken identifier (UUIDv7). | 

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
**204** | BootstrapToken revoked. |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to revoke BootstrapTokens in the target Project. Body is a &#x60;PermissionDenied&#x60; problem .  |  -  |
**404** | BootstrapToken or Project not found. |  -  |
**409** | BootstrapToken has already been consumed or revoked — the redemption window is closed and a fresh token must be issued.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

