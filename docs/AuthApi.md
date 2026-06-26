# plexsphere.AuthApi

All URIs are relative to *http://localhost*

Method | HTTP request | Description
------------- | ------------- | -------------
[**delete_auth_session**](AuthApi.md#delete_auth_session) | **DELETE** /v1/auth/whoami | Sign the current caller out.
[**delete_auth_token_by_id**](AuthApi.md#delete_auth_token_by_id) | **DELETE** /v1/auth/tokens/{id} | Immediately revoke an API token.
[**get_auth_callback**](AuthApi.md#get_auth_callback) | **GET** /v1/auth/callback | OIDC redirect callback — exchanges &#x60;code&#x60; for a session.
[**get_auth_id_p_bindings**](AuthApi.md#get_auth_id_p_bindings) | **GET** /v1/auth/idp-bindings | List a Domain&#39;s effective IdP bindings for the sign-in chooser.
[**get_auth_tokens**](AuthApi.md#get_auth_tokens) | **GET** /v1/auth/tokens | List the caller&#39;s API tokens (no plaintext).
[**get_auth_whoami**](AuthApi.md#get_auth_whoami) | **GET** /v1/auth/whoami | Describe the authenticated principal.
[**post_auth_device_approve**](AuthApi.md#post_auth_device_approve) | **POST** /v1/auth/device/approve | Approve a pending RFC 8628 device-authorization session.
[**post_auth_device_code**](AuthApi.md#post_auth_device_code) | **POST** /v1/auth/device-code | Initiate an RFC 8628 device-authorization flow.
[**post_auth_device_token**](AuthApi.md#post_auth_device_token) | **POST** /v1/auth/device-token | Poll for the RFC 8628 device-authorization result.
[**post_auth_service_token**](AuthApi.md#post_auth_service_token) | **POST** /v1/auth/service/token | Issue an access token for a ServiceIdentity.
[**post_auth_sign_in**](AuthApi.md#post_auth_sign_in) | **POST** /v1/auth/sign-in | Initiate an end-user OIDC sign-in (authorization code + PKCE).
[**post_auth_sign_out_global**](AuthApi.md#post_auth_sign_out_global) | **POST** /v1/auth/sign-out/global | Sign out of every active session for the caller.
[**post_auth_token_rotate**](AuthApi.md#post_auth_token_rotate) | **POST** /v1/auth/tokens/{id}/rotate | Rotate an API token; old plaintext is retired with Sunset.
[**post_auth_tokens**](AuthApi.md#post_auth_tokens) | **POST** /v1/auth/tokens | Issue a new API token (psk-shaped) for the caller identity.


# **delete_auth_session**
> delete_auth_session()

Sign the current caller out.

Terminates the caller's authenticated session and clears the
session cookie. The endpoint is
idempotent and unauthenticated: callers without a session
cookie, or with a stale or already-revoked one, still receive
a 204 with the cookie cleared. No information about whether a
session existed is disclosed in the response body. Because the
handler treats every caller as a successful 204 by design (the
four behaviour scenarios are documented on
`internal/transport/http/v1/auth/signout.go`), this operation
advertises only the 204 response — no 401 or 500 path is
reachable from the handler (-1 Q4).


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
    api_instance = plexsphere.AuthApi(api_client)

    try:
        # Sign the current caller out.
        api_instance.delete_auth_session()
    except Exception as e:
        print("Exception when calling AuthApi->delete_auth_session: %s\n" % e)
```



### Parameters

This endpoint does not need any parameter.

### Return type

void (empty response body)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: Not defined

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**204** | Session terminated (or no active session existed). |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **delete_auth_token_by_id**
> delete_auth_token_by_id(id)

Immediately revoke an API token.

Revokes the token identified by `id` with no grace period
. Subsequent authentications with the
plaintext fail closed.


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
    api_instance = plexsphere.AuthApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Token identifier (UUIDv7).

    try:
        # Immediately revoke an API token.
        api_instance.delete_auth_token_by_id(id)
    except Exception as e:
        print("Exception when calling AuthApi->delete_auth_token_by_id: %s\n" % e)
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
**404** | Token not found (or does not belong to caller). |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **get_auth_callback**
> get_auth_callback(code, state)

OIDC redirect callback — exchanges `code` for a session.

Receives the OIDC redirect after the end user completes sign-in
at the IdP. Validates `state` and `nonce`, exchanges the
authorization code for tokens, provisions/updates the user via
the JIT policy, issues a plexsphere session
cookie, and 303 See Other-redirects the browser to the application root
(`/`). The endpoint is invoked exclusively via top-level
browser navigation per RFC 6749 §4.1.2; clients that need a
machine-readable session shape should call `GET
/v1/auth/whoami` once the session cookie is set
.

Failure responses are content-negotiated via the `Accept`
request header:

- **Browser-leg failure** — when `Accept` is absent, `*/*`, or
  includes `text/html`, the server responds with `303 See
  Other` to `/?auth_error_kind=...&auth_error_status=...&auth_error_detail=...`
  so the browser client can render the error inline next to the sign-in
  form. No session cookie is issued.
- **JSON-leg failure** — when `Accept` includes
  `application/json` or `application/problem+json`, the server
  returns the original status (`400`, `500`, or `502`) with an
  `application/problem+json` body conforming to RFC 7807.


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
    api_instance = plexsphere.AuthApi(api_client)
    code = 'code_example' # str | Authorization code issued by the IdP.
    state = 'state_example' # str | Opaque value used to correlate the redirect.

    try:
        # OIDC redirect callback — exchanges `code` for a session.
        api_instance.get_auth_callback(code, state)
    except Exception as e:
        print("Exception when calling AuthApi->get_auth_callback: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **code** | **str**| Authorization code issued by the IdP. | 
 **state** | **str**| Opaque value used to correlate the redirect. | 

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
**303** | Session established; the response carries the &#x60;plexsphere_session&#x60; cookie and a &#x60;Location: /&#x60; redirect so the browser lands on the application root. The cookie is issued with &#x60;Path&#x3D;/v1/&#x60;, &#x60;HttpOnly&#x60;, and &#x60;SameSite&#x3D;Strict&#x60;; the &#x60;Secure&#x60; attribute is set when the request was served over TLS or when &#x60;X-Forwarded-Proto: https&#x60; was honoured via &#x60;PLEXSPHERE_AUTH_TRUST_PROXY_HEADERS&#x60;. The same status is also used for browser-leg failure redirects to &#x60;/?auth_error_kind&#x3D;...&amp;auth_error_status&#x3D;...&amp;auth_error_detail&#x3D;...&#x60;, in which case no &#x60;Set-Cookie&#x60; is emitted.  |  * Location - Absolute or root-relative URL the browser must follow. <br>  * Set-Cookie - Issues the &#x60;plexsphere_session&#x60; cookie (success path only). <br>  |
**400** | state/nonce mismatch, expired verifier, or token exchange rejected by the IdP. Returned only on the JSON-leg; the browser-leg surfaces this as a 303 redirect carrying &#x60;auth_error_kind&#x60; and &#x60;auth_error_status&#x3D;400&#x60; query parameters.  |  -  |
**500** | Internal server error while completing the callback. Returned only on the JSON-leg; the browser-leg surfaces this as a 303 redirect carrying &#x60;auth_error_status&#x3D;500&#x60; .  |  -  |
**502** | Upstream IdP failure during discovery, token exchange, or id_token verification. Returned only on the JSON-leg; the browser-leg surfaces this as a 303 redirect carrying &#x60;auth_error_status&#x3D;502&#x60;.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **get_auth_id_p_bindings**
> List[DomainIdPBinding] get_auth_id_p_bindings(domain_id)

List a Domain's effective IdP bindings for the sign-in chooser.

Unauthenticated, display-safe discovery surface the dashboard
sign-in page calls once a Domain id is known (typed or supplied
via a deep link) so it can render a provider chooser. Returns
only the active bindings the Domain can sign in with — its own
active bindings, falling back to the platform-scoped shared
bindings when the Domain owns none — projected to non-secret
fields and ordered primary-first.

The endpoint is intentionally a pre-auth enumeration surface and
does not behave as a Domain-existence oracle: a Domain that does
not exist returns the same shape (the applicable platform
bindings, or an empty list) as a Domain that exists but owns no
bindings, so an unauthenticated caller cannot distinguish the
two. Responses are marked `Cache-Control: no-store`.


### Example


```python
import plexsphere
from plexsphere.models.domain_id_p_binding import DomainIdPBinding
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
    api_instance = plexsphere.AuthApi(api_client)
    domain_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Domain whose effective IdP bindings to list.

    try:
        # List a Domain's effective IdP bindings for the sign-in chooser.
        api_response = api_instance.get_auth_id_p_bindings(domain_id)
        print("The response of AuthApi->get_auth_id_p_bindings:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling AuthApi->get_auth_id_p_bindings: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **domain_id** | **UUID**| Domain whose effective IdP bindings to list. | 

### Return type

[**List[DomainIdPBinding]**](DomainIdPBinding.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | The Domain&#39;s effective active bindings, display-safe. |  -  |
**400** | The domain_id query parameter is missing or not a UUID. |  -  |
**500** | Internal server error while listing the bindings. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **get_auth_tokens**
> List[APITokenSummary] get_auth_tokens()

List the caller's API tokens (no plaintext).

Returns the caller's API-token summaries. Plaintext is never
included — it is only available at issuance time
.


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
    api_instance = plexsphere.AuthApi(api_client)

    try:
        # List the caller's API tokens (no plaintext).
        api_response = api_instance.get_auth_tokens()
        print("The response of AuthApi->get_auth_tokens:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling AuthApi->get_auth_tokens: %s\n" % e)
```



### Parameters

This endpoint does not need any parameter.

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
**200** | List of token summaries. |  -  |
**401** | Caller is not authenticated. |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **get_auth_whoami**
> Whoami get_auth_whoami()

Describe the authenticated principal.

Returns the resolved principal metadata for the current request
— useful both for debugging and for CLIs that need to render a
"you are signed in as" banner.


### Example


```python
import plexsphere
from plexsphere.models.whoami import Whoami
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
    api_instance = plexsphere.AuthApi(api_client)

    try:
        # Describe the authenticated principal.
        api_response = api_instance.get_auth_whoami()
        print("The response of AuthApi->get_auth_whoami:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling AuthApi->get_auth_whoami: %s\n" % e)
```



### Parameters

This endpoint does not need any parameter.

### Return type

[**Whoami**](Whoami.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Principal description. |  -  |
**401** | Caller is not authenticated. |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **post_auth_device_approve**
> DeviceApproveResponse post_auth_device_approve(device_approve_request)

Approve a pending RFC 8628 device-authorization session.

Flips a pending `DeviceSession` (looked up by the operator-typed
`user_code`) to `status=approved` and pins the authenticated
principal onto it so the next `POST /v1/auth/device-token` poll
mints a token bound to that user.

Authentication: requires the `plexsphere_session` cookie minted
by the OIDC callback. The CSRF middleware additionally requires
the `X-Plexsphere-CSRF` header to echo the `plexsphere_csrf`
cookie value (issue #181).

Errors use the RFC 8628-adjacent taxonomy in `Problem.code`:
`device-code-not-found` (404 — unknown user_code),
`device-code-expired` (409 — session past `expires_at`),
`device-code-already-approved` (409 — already approved by an
earlier request), `csrf-token-mismatch` /
`csrf-origin-mismatch` (403 — see /v1/auth/sign-in for the
taxonomy).


### Example


```python
import plexsphere
from plexsphere.models.device_approve_request import DeviceApproveRequest
from plexsphere.models.device_approve_response import DeviceApproveResponse
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
    api_instance = plexsphere.AuthApi(api_client)
    device_approve_request = plexsphere.DeviceApproveRequest() # DeviceApproveRequest | 

    try:
        # Approve a pending RFC 8628 device-authorization session.
        api_response = api_instance.post_auth_device_approve(device_approve_request)
        print("The response of AuthApi->post_auth_device_approve:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling AuthApi->post_auth_device_approve: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **device_approve_request** | [**DeviceApproveRequest**](DeviceApproveRequest.md)|  | 

### Return type

[**DeviceApproveResponse**](DeviceApproveResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Device session approved; the polling client now mints a token. |  -  |
**400** | Invalid request body (missing or malformed user_code). |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | CSRF check failed — Problem.code carries the &#x60;csrf-token-mismatch&#x60; / &#x60;csrf-origin-mismatch&#x60; / &#x60;csrf-origin-not-configured&#x60; taxonomy.  |  -  |
**404** | No device session matches the supplied &#x60;user_code&#x60; (Problem.code &#x3D; &#x60;device-code-not-found&#x60;).  |  -  |
**409** | Session is already approved or expired (Problem.code &#x3D; &#x60;device-code-already-approved&#x60; / &#x60;device-code-expired&#x60;).  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **post_auth_device_code**
> DeviceCodeResponse post_auth_device_code(device_code_request)

Initiate an RFC 8628 device-authorization flow.

Issues a `device_code` / `user_code` pair the CLI can present to
the end user so they can authenticate in a browser on a second
device (RFC 8628). The client then polls
/v1/auth/device-token.


### Example


```python
import plexsphere
from plexsphere.models.device_code_request import DeviceCodeRequest
from plexsphere.models.device_code_response import DeviceCodeResponse
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
    api_instance = plexsphere.AuthApi(api_client)
    device_code_request = plexsphere.DeviceCodeRequest() # DeviceCodeRequest | 

    try:
        # Initiate an RFC 8628 device-authorization flow.
        api_response = api_instance.post_auth_device_code(device_code_request)
        print("The response of AuthApi->post_auth_device_code:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling AuthApi->post_auth_device_code: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **device_code_request** | [**DeviceCodeRequest**](DeviceCodeRequest.md)|  | 

### Return type

[**DeviceCodeResponse**](DeviceCodeResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Device-code issued. |  -  |
**400** | Invalid request body (missing domain or binding). |  -  |
**404** | No active IdP binding for the requested Domain. |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **post_auth_device_token**
> ServiceTokenResponse post_auth_device_token(device_token_request)

Poll for the RFC 8628 device-authorization result.

Exchanges a `device_code` for an access token once the user has
completed the browser flow. Errors use the RFC 8628 taxonomy
(`authorization_pending`, `slow_down`, `access_denied`,
`expired_token`, `invalid_grant`) surfaced in the Problem
body's `code` field.


### Example


```python
import plexsphere
from plexsphere.models.device_token_request import DeviceTokenRequest
from plexsphere.models.service_token_response import ServiceTokenResponse
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
    api_instance = plexsphere.AuthApi(api_client)
    device_token_request = plexsphere.DeviceTokenRequest() # DeviceTokenRequest | 

    try:
        # Poll for the RFC 8628 device-authorization result.
        api_response = api_instance.post_auth_device_token(device_token_request)
        print("The response of AuthApi->post_auth_device_token:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling AuthApi->post_auth_device_token: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **device_token_request** | [**DeviceTokenRequest**](DeviceTokenRequest.md)|  | 

### Return type

[**ServiceTokenResponse**](ServiceTokenResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Access token issued; the flow is complete. |  -  |
**400** | RFC 8628 error — Problem.code carries the taxonomy value (&#x60;authorization_pending&#x60;, &#x60;slow_down&#x60;, &#x60;access_denied&#x60;, &#x60;expired_token&#x60;, &#x60;invalid_grant&#x60;).  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **post_auth_service_token**
> ServiceTokenResponse post_auth_service_token(service_token_request)

Issue an access token for a ServiceIdentity.

Mints an access token for a registered ServiceIdentity using
either `client_credentials` (RFC 6749 §4.4) or
`urn:ietf:params:oauth:grant-type:jwt-bearer` (RFC 7523) with
a SPIFFE JWT-SVID. The grant must match the
ServiceIdentity's `federation_kind`; a mismatch returns
`403 Problem{code: federation_kind_mismatch}` (also raised
when the verified SPIFFE assertion's `sub` does not match
the ServiceIdentity's `Subject`). The JWT-bearer path
verifies the assertion against the IdPBinding's SPIFFE trust
bundle and emits one `identity.ServiceIdentityAuthenticated`
outbox row alongside the bearer mint, matching the
client_credentials branch.


### Example


```python
import plexsphere
from plexsphere.models.service_token_request import ServiceTokenRequest
from plexsphere.models.service_token_response import ServiceTokenResponse
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
    api_instance = plexsphere.AuthApi(api_client)
    service_token_request = plexsphere.ServiceTokenRequest() # ServiceTokenRequest | 

    try:
        # Issue an access token for a ServiceIdentity.
        api_response = api_instance.post_auth_service_token(service_token_request)
        print("The response of AuthApi->post_auth_service_token:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling AuthApi->post_auth_service_token: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **service_token_request** | [**ServiceTokenRequest**](ServiceTokenRequest.md)|  | 

### Return type

[**ServiceTokenResponse**](ServiceTokenResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Access token issued. |  -  |
**400** | Malformed request, unsupported grant_type, or missing required field for the chosen grant.  |  -  |
**401** | Client authentication failed. &#x60;Problem.code&#x60; is one of: &#x60;invalid_client&#x60; (unknown client_id), or &#x60;spiffe_verification_failed&#x60; (SPIFFE JWT-SVID signature, expiry, issuer, or audience did not validate — includes unknown-&#x60;kid&#x60;-after-one-refresh).  |  -  |
**403** | &#x60;Problem.code &#x3D; federation_kind_mismatch&#x60;. Either the grant type does not match the ServiceIdentity&#39;s &#x60;federation_kind&#x60;, or the verified SPIFFE assertion&#39;s &#x60;sub&#x60; claim does not match the resolved ServiceIdentity&#39;s &#x60;Subject&#x60;.  |  -  |
**501** | &#x60;Problem.code &#x3D; not_provisioned&#x60;. Returned by the jwt-bearer branch when the deployment has not wired a SPIFFE verifier; the federation_kind gate already passed so the failure is observably verifier-side.  |  -  |
**502** | Upstream verification failure. &#x60;Problem.code&#x60; is one of: &#x60;invalid_client&#x60; (upstream IdP rejected client_credentials), &#x60;spiffe_bundle_unreachable&#x60; (SPIFFE trust bundle endpoint failed and no cached copy was available), or &#x60;spiffe_bundle_unconfigured&#x60; (the IdPBinding has no &#x60;spiffe_bundle_url&#x60; recorded).  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **post_auth_sign_in**
> SignInResponse post_auth_sign_in(sign_in_request)

Initiate an end-user OIDC sign-in (authorization code + PKCE).

Starts the OIDC authorization-code + PKCE flow against the
resolved per-Domain IdP binding. Returns the
authorization URL the caller should redirect to plus the opaque
state/code-verifier handle that /v1/auth/callback consumes to
finish the exchange.


### Example


```python
import plexsphere
from plexsphere.models.sign_in_request import SignInRequest
from plexsphere.models.sign_in_response import SignInResponse
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
    api_instance = plexsphere.AuthApi(api_client)
    sign_in_request = plexsphere.SignInRequest() # SignInRequest | 

    try:
        # Initiate an end-user OIDC sign-in (authorization code + PKCE).
        api_response = api_instance.post_auth_sign_in(sign_in_request)
        print("The response of AuthApi->post_auth_sign_in:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling AuthApi->post_auth_sign_in: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **sign_in_request** | [**SignInRequest**](SignInRequest.md)|  | 

### Return type

[**SignInResponse**](SignInResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Authorization URL and opaque handles to complete the flow. |  -  |
**400** | Invalid request body (missing domain / binding resolution failed). |  -  |
**404** | No active IdP binding found for the requested Domain. |  -  |
**500** | Internal server error while preparing the flow. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **post_auth_sign_out_global**
> post_auth_sign_out_global()

Sign out of every active session for the caller.

Revokes every server-side `plexsphere_session` bound to the
authenticated user (regardless of which device originally
issued it) and clears the current cookie. Intended for the
dashboard's "Sign out everywhere" affordance and for the
compromise / password-reset / IdP-driven force-logout paths.

The endpoint is authenticated: callers without an interactive
cookie receive `401 Unauthorized`. An `api_token` principal —
a `psk_`-prefixed cookie value — is rejected with `403
Forbidden` because the cross-credential boundary forbids a
leaked API token from bulk-revoking its owner's interactive
sessions. The cookie-clearing `Set-Cookie` header is emitted
on every response (including the 401 / 403 paths) so a stale
browser tab drops whatever value it held.

On success the response body is empty and the audit pipeline
records a single `identity.UserSignedOut` event whose payload
carries `cookie_cleared=true`, `sessions_revoked=N` (the count
of evicted server-side sessions, possibly 0 on a re-click
after the first response landed) and `reason=user-initiated`.


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
    api_instance = plexsphere.AuthApi(api_client)

    try:
        # Sign out of every active session for the caller.
        api_instance.post_auth_sign_out_global()
    except Exception as e:
        print("Exception when calling AuthApi->post_auth_sign_out_global: %s\n" % e)
```



### Parameters

This endpoint does not need any parameter.

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
**204** | Every server-side session for the caller has been invalidated and the response carries the canonical cookie-clearing &#x60;Set-Cookie&#x60; header.  |  -  |
**401** | Caller is not authenticated. The response still clears the session cookie so a stale browser tab drops its copy.  |  -  |
**403** | Caller authenticated as an &#x60;api_token&#x60; principal (the &#x60;plexsphere_session&#x60; cookie value carries the &#x60;psk_&#x60; prefix). Global sign-out is reserved for interactive sessions; revoke the API token via &#x60;DELETE /v1/auth/tokens/{id}&#x60; instead.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **post_auth_token_rotate**
> APITokenRotateResponse post_auth_token_rotate(id)

Rotate an API token; old plaintext is retired with Sunset.

Issues a replacement plaintext for the named token and marks the
previous plaintext for retirement. A `Sunset` header names the
RFC 8594 retirement deadline of the rotated-from token
.


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
    api_instance = plexsphere.AuthApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Token identifier (UUIDv7).

    try:
        # Rotate an API token; old plaintext is retired with Sunset.
        api_response = api_instance.post_auth_token_rotate(id)
        print("The response of AuthApi->post_auth_token_rotate:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling AuthApi->post_auth_token_rotate: %s\n" % e)
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
**404** | Token not found (or does not belong to caller). |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **post_auth_tokens**
> APITokenIssueResponse post_auth_tokens(api_token_issue_request)

Issue a new API token (psk-shaped) for the caller identity.

Mints a psk-shaped API token bound to the supplied identity
reference. The plaintext is returned exactly
once in this response; subsequent reads surface only summary
metadata.


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
    api_instance = plexsphere.AuthApi(api_client)
    api_token_issue_request = plexsphere.APITokenIssueRequest() # APITokenIssueRequest | 

    try:
        # Issue a new API token (psk-shaped) for the caller identity.
        api_response = api_instance.post_auth_tokens(api_token_issue_request)
        print("The response of AuthApi->post_auth_tokens:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling AuthApi->post_auth_tokens: %s\n" % e)
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
**201** | Token issued; plaintext is included exactly once. |  * Sunset - RFC 8594 sunset timestamp for the retired plaintext. <br>  |
**400** | Invalid body (bad env_prefix, missing identity_ref). |  -  |
**401** | Caller is not authenticated. |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

