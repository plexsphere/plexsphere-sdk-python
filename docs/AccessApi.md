# plexsphere.AccessApi

All URIs are relative to *http://localhost*

Method | HTTP request | Description
------------- | ------------- | -------------
[**attach_session**](AccessApi.md#attach_session) | **GET** /v1/projects/{project_id}/sessions/{session_id}/attach | Attach to a live SSH session as a bidirectional WebSocket stream.
[**get_session**](AccessApi.md#get_session) | **GET** /v1/projects/{project_id}/sessions/{session_id} | Inspect a single mediated session.
[**issue_session**](AccessApi.md#issue_session) | **POST** /v1/projects/{project_id}/sessions | Issue a short-lived, session-scoped JWT for a mediated target.
[**list_sessions**](AccessApi.md#list_sessions) | **GET** /v1/projects/{project_id}/sessions | List mediated sessions for a Project.
[**post_node_session_callback**](AccessApi.md#post_node_session_callback) | **POST** /v1/nodes/{id}/sessions/{session_id} | Report per-kind session activity from a Node.
[**revoke_session**](AccessApi.md#revoke_session) | **POST** /v1/projects/{project_id}/sessions/{session_id}:revoke | Instantly revoke a live mediated session.


# **attach_session**
> attach_session(project_id, session_id)

Attach to a live SSH session as a bidirectional WebSocket stream.

Upgrades the request to a WebSocket and bridges the browser to the
mediated SSH session identified by `{session_id}` within the owning
Project. The handler re-runs the issuance-equivalent ReBAC check on
the session's Resource BEFORE dialing anything, validates the Session
is `active` and `ssh`-kind, then dials the target plexd
`listener_endpoint` over the mesh and copies bytes bidirectionally
between the client socket and the mesh connection.

This operation does NOT speak JSON. On a successful upgrade the
response is `101 Switching Protocols` and the connection body is an
opaque, bidirectional byte stream carrying the SSH transport — the
request and response bodies are NOT JSON documents. The
`listener_endpoint` is never written to the client; the browser only
ever sees the proxied stream. Error paths (before the upgrade
completes) follow the RFC 9457 `application/problem+json` envelope.


### Example

* Bearer (JWT) Authentication (operatorBearer):

```python
import plexsphere
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
    api_instance = plexsphere.AccessApi(api_client)
    project_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Project. The Access Orchestrator issue, list, read, and revoke operations are scoped per Project, so every operator-facing endpoint requires the Project identifier in the path. 
    session_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Session identifier (UUIDv7). Bound on `/v1/projects/{project_id}/sessions/{session_id}`, its `:revoke` sub-resource, and its `/attach` WebSocket gateway for the single-Session read, revoke, and attach. 

    try:
        # Attach to a live SSH session as a bidirectional WebSocket stream.
        api_instance.attach_session(project_id, session_id)
    except Exception as e:
        print("Exception when calling AccessApi->attach_session: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **project_id** | **UUID**| Owning Project. The Access Orchestrator issue, list, read, and revoke operations are scoped per Project, so every operator-facing endpoint requires the Project identifier in the path.  | 
 **session_id** | **UUID**| Session identifier (UUIDv7). Bound on &#x60;/v1/projects/{project_id}/sessions/{session_id}&#x60;, its &#x60;:revoke&#x60; sub-resource, and its &#x60;/attach&#x60; WebSocket gateway for the single-Session read, revoke, and attach.  | 

### Return type

void (empty response body)

### Authorization

[operatorBearer](../README.md#operatorBearer)

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**101** | Switching Protocols. The connection is upgraded to a WebSocket and the body becomes an opaque, bidirectional byte stream carrying the SSH transport — it is NOT a JSON document. The gateway copies bytes between the client and the mesh connection until either side closes or the request context is cancelled.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to attach to this Session&#39;s Resource. Body is a &#x60;PermissionDenied&#x60; problem; the gateway closes the socket with WebSocket close code &#x60;1008&#x60; (policy violation) when the denial is detected after the upgrade.  |  -  |
**404** | The owning Project or the addressed Session does not exist or is not visible. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;project_not_found&#x60;, &#x60;session_not_found&#x60; }.  |  -  |
**409** | The addressed Session cannot be attached. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;session_not_active&#x60; (the Session is revoked, expired, or otherwise not live), &#x60;session_not_ssh&#x60; (the Session is &#x60;k8s&#x60;- or &#x60;tcp&#x60;-kind — only the &#x60;ssh&#x60; kind has a browser terminal) }. The gateway rejects with this status BEFORE any mesh dial is attempted.  |  -  |
**426** | Upgrade Required. The request did not carry the WebSocket upgrade handshake headers, so the gateway refuses to proceed. Body is a &#x60;Problem&#x60; with &#x60;code: upgrade_required&#x60; and the response advertises &#x60;Connection: Upgrade&#x60; / &#x60;Upgrade: websocket&#x60;.  |  * Upgrade - Advertises the protocol the client must upgrade to (&#x60;websocket&#x60;).  <br>  |
**501** | The access attach gateway is not yet provisioned in this build. The Problem body&#39;s &#x60;code&#x60; field is &#x60;access_attach_not_provisioned&#x60; so log scrapers can alert on the deferred-wiring state. Resolves once the session-lookup and mesh-dialer ports are wired into the v1 handler dependency cone.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **get_session**
> Session get_session(project_id, session_id)

Inspect a single mediated session.

Returns the metadata-only projection of the Session identified by
`{session_id}` within the owning Project. The handler runs the
session-read ReBAC check BEFORE the persistence read. The
projection never carries the signed token — the token is delivered
only once, on issuance.


### Example

* Bearer (JWT) Authentication (operatorBearer):

```python
import plexsphere
from plexsphere.models.session import Session
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
    api_instance = plexsphere.AccessApi(api_client)
    project_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Project. The Access Orchestrator issue, list, read, and revoke operations are scoped per Project, so every operator-facing endpoint requires the Project identifier in the path. 
    session_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Session identifier (UUIDv7). Bound on `/v1/projects/{project_id}/sessions/{session_id}`, its `:revoke` sub-resource, and its `/attach` WebSocket gateway for the single-Session read, revoke, and attach. 

    try:
        # Inspect a single mediated session.
        api_response = api_instance.get_session(project_id, session_id)
        print("The response of AccessApi->get_session:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling AccessApi->get_session: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **project_id** | **UUID**| Owning Project. The Access Orchestrator issue, list, read, and revoke operations are scoped per Project, so every operator-facing endpoint requires the Project identifier in the path.  | 
 **session_id** | **UUID**| Session identifier (UUIDv7). Bound on &#x60;/v1/projects/{project_id}/sessions/{session_id}&#x60;, its &#x60;:revoke&#x60; sub-resource, and its &#x60;/attach&#x60; WebSocket gateway for the single-Session read, revoke, and attach.  | 

### Return type

[**Session**](Session.md)

### Authorization

[operatorBearer](../README.md#operatorBearer)

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | The requested Session. |  -  |
**400** | Malformed id. Body is a &#x60;Problem&#x60; with &#x60;code: invalid_session_id&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to read this Session. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Session not found. Body is a &#x60;Problem&#x60; with &#x60;code: session_not_found&#x60;.  |  -  |
**501** | The access-orchestrator surface is not yet provisioned in this build. The Problem body&#39;s &#x60;code&#x60; field is &#x60;access_session_not_provisioned&#x60;.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **issue_session**
> IssuedSession issue_session(project_id, issue_session_request)

Issue a short-lived, session-scoped JWT for a mediated target.

Issues a short-lived, session-scoped EdDSA JWT for one mediated
target inside the owning Project. The handler runs the issuance
ReBAC check on the targeted Resource BEFORE any persistence write,
enforces the per-Domain step-up policy, clamps the requested TTL to
the per-Domain ceiling, admits the session against the per-Domain
concurrency and rate budgets, then mints one Session whose JTI is
its own identifier and signs the canonical claim bytes with the
per-Domain Signing Service key.

The request body supplies the targeted `resource_id`, the session
`kind` (`ssh`, `k8s`, or `tcp`), the matching per-kind `target`,
an optional `requested_ttl_seconds` (silently clamped to the
per-Domain maximum), and an optional `idempotency_key` (a repeat
within the dedupe window returns the original session and token).

The signed token is returned EXACTLY ONCE, in this response. It is
never persisted server-side beyond its identifier and expiry, and
the list and read projections never re-expose it.


### Example

* Bearer (JWT) Authentication (operatorBearer):

```python
import plexsphere
from plexsphere.models.issue_session_request import IssueSessionRequest
from plexsphere.models.issued_session import IssuedSession
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
    api_instance = plexsphere.AccessApi(api_client)
    project_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Project. The Access Orchestrator issue, list, read, and revoke operations are scoped per Project, so every operator-facing endpoint requires the Project identifier in the path. 
    issue_session_request = {"resource_id":"0190a8b8-a0c0-7a0a-8a0a-a0a0a0a0a0e1","kind":"ssh","target":{"ssh":{"user":"incident-responder","allowed_commands":["journalctl","systemctl status"]}},"requested_ttl_seconds":1800} # IssueSessionRequest | 

    try:
        # Issue a short-lived, session-scoped JWT for a mediated target.
        api_response = api_instance.issue_session(project_id, issue_session_request)
        print("The response of AccessApi->issue_session:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling AccessApi->issue_session: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **project_id** | **UUID**| Owning Project. The Access Orchestrator issue, list, read, and revoke operations are scoped per Project, so every operator-facing endpoint requires the Project identifier in the path.  | 
 **issue_session_request** | [**IssueSessionRequest**](IssueSessionRequest.md)|  | 

### Return type

[**IssuedSession**](IssuedSession.md)

### Authorization

[operatorBearer](../README.md#operatorBearer)

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**201** | Session issued. Body carries the metadata-only Session projection, the signed EdDSA JWT (delivered exactly once), the per-kind plexd listener endpoint, and the expiry. The &#x60;Location&#x60; header points at the single-Session read path.  |  * Location - Absolute path of the issued Session for the single-Session read.  <br>  |
**400** | Invalid request — typically a malformed body, a &#x60;kind&#x60; that does not match the supplied &#x60;target&#x60; variant, or a per-kind target that fails a domain invariant. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;invalid_body&#x60;, &#x60;invalid_target&#x60; }.  |  -  |
**401** | Caller is not authenticated, OR the targeted kind requires a fresh step-up the caller has not satisfied. On the step-up path the body is a &#x60;Problem&#x60; with &#x60;code: step_up_required&#x60; and the response carries an RFC 9470 &#x60;WWW-Authenticate: Bearer error&#x3D;\&quot;insufficient_user_authentication\&quot;&#x60; challenge — with an &#x60;acr_values&#x60; parameter naming the required assurance set when the unmet requirement is an ACR, or the bare challenge when the unmet requirement is an AMR.  |  * WWW-Authenticate - RFC 9470 step-up challenge, present only on the step-up-required path.  <br>  |
**403** | Caller is not authorized to issue sessions on the targeted Resource, or the target lies outside the caller&#39;s authorised scope. Body is a &#x60;PermissionDenied&#x60; problem whose &#x60;reason&#x60; is &#x60;insufficient_relation&#x60; or &#x60;out_of_scope&#x60;.  |  -  |
**404** | The owning Project or the targeted Resource does not exist or is not visible. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;project_not_found&#x60;, &#x60;resource_not_found&#x60; }.  |  -  |
**409** | The issuance could not be admitted. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;session_limit_exceeded&#x60; (a per-Domain concurrency cap or the issuance rate limit is full — the breached dimension is named in &#x60;detail&#x60;), &#x60;idempotent_replay&#x60; (a prior request with the same idempotency key is still in flight) }.  |  -  |
**501** | The access-orchestrator surface is not yet provisioned in this build. The Problem body&#39;s &#x60;code&#x60; field is &#x60;access_session_not_provisioned&#x60; so log scrapers can alert on the deferred-wiring state. Resolves once the issuance service and its signer / authz ports are wired into the v1 handler dependency cone.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **list_sessions**
> SessionList list_sessions(project_id, status=status, kind=kind, identity_id=identity_id, cursor=cursor, limit=limit)

List mediated sessions for a Project.

Returns an issuance-ordered page of Session lifecycle metadata for
the owning Project, optionally narrowed by aggregate `status`,
`kind`, or `identity_id`. The handler runs the session-read ReBAC
check BEFORE the persistence read. The projection is metadata-only
— it carries the session identity, the aggregate status, and the
per-kind target, but never the signed token.

The pagination cursor is opaque and HMAC-signed by the server, so
a tampered cursor surfaces as `400` on the next call.


### Example

* Bearer (JWT) Authentication (operatorBearer):

```python
import plexsphere
from plexsphere.models.session_kind import SessionKind
from plexsphere.models.session_list import SessionList
from plexsphere.models.session_status import SessionStatus
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
    api_instance = plexsphere.AccessApi(api_client)
    project_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Project. The Access Orchestrator issue, list, read, and revoke operations are scoped per Project, so every operator-facing endpoint requires the Project identifier in the path. 
    status = plexsphere.SessionStatus() # SessionStatus | Optional aggregate-status filter. When present, only Sessions whose status equals the named value are returned.  (optional)
    kind = plexsphere.SessionKind() # SessionKind | Optional kind filter. When present, only Sessions of the named kind are returned.  (optional)
    identity_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Optional identity filter. When present, only Sessions issued to the named Identity are returned.  (optional)
    cursor = 'cursor_example' # str | Opaque continuation token returned by a previous call's `next_cursor`. The encoding is HMAC-signed by the server so a tampered cursor surfaces as `400`.  (optional)
    limit = 50 # int | Maximum number of items to return in a single page. The handler clamps the value to [1, 200] before forwarding it to the read service.  (optional) (default to 50)

    try:
        # List mediated sessions for a Project.
        api_response = api_instance.list_sessions(project_id, status=status, kind=kind, identity_id=identity_id, cursor=cursor, limit=limit)
        print("The response of AccessApi->list_sessions:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling AccessApi->list_sessions: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **project_id** | **UUID**| Owning Project. The Access Orchestrator issue, list, read, and revoke operations are scoped per Project, so every operator-facing endpoint requires the Project identifier in the path.  | 
 **status** | [**SessionStatus**](.md)| Optional aggregate-status filter. When present, only Sessions whose status equals the named value are returned.  | [optional] 
 **kind** | [**SessionKind**](.md)| Optional kind filter. When present, only Sessions of the named kind are returned.  | [optional] 
 **identity_id** | **UUID**| Optional identity filter. When present, only Sessions issued to the named Identity are returned.  | [optional] 
 **cursor** | **str**| Opaque continuation token returned by a previous call&#39;s &#x60;next_cursor&#x60;. The encoding is HMAC-signed by the server so a tampered cursor surfaces as &#x60;400&#x60;.  | [optional] 
 **limit** | **int**| Maximum number of items to return in a single page. The handler clamps the value to [1, 200] before forwarding it to the read service.  | [optional] [default to 50]

### Return type

[**SessionList**](SessionList.md)

### Authorization

[operatorBearer](../README.md#operatorBearer)

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Page of Sessions. |  -  |
**400** | Invalid query parameters — typically a tampered or malformed cursor (&#x60;code: invalid_cursor&#x60;), an out-of-range &#x60;limit&#x60; (&#x60;code: invalid_limit&#x60;), or a malformed &#x60;status&#x60; / &#x60;kind&#x60; filter.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to read Sessions in the owning Project. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | The owning Project does not exist or is not visible. Body is a &#x60;Problem&#x60; with &#x60;code: project_not_found&#x60;.  |  -  |
**501** | The access-orchestrator surface is not yet provisioned in this build. The Problem body&#39;s &#x60;code&#x60; field is &#x60;access_session_not_provisioned&#x60;.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **post_node_session_callback**
> post_node_session_callback(id, session_id, authorization, session_activity_request)

Report per-kind session activity from a Node.

Accepts a per-kind session-activity callback from the target plexd
and appends it to the session's append-only activity log, touching
the session's last-active timestamp so the idle-timeout sweeper
sees progress. The handler:

  1. Authenticates the caller against the Node Secret Key (NSK)
     plaintext supplied in the `Authorization: Bearer` header,
     rejecting revoked credentials with 401.
  2. Asserts that the NSK belongs to the Node addressed by the
     path `id`, refusing cross-Node use with 403
     `nsk_node_mismatch` so a leaked NSK cannot be replayed
     against a sibling Node's session.
  3. Appends the reported per-kind activity row. An `ssh` session
     records `command_executed` / `command_exited` activity; a
     `k8s` session records `api_request` activity; a `tcp` session
     records exactly a `session_started` and a `session_ended`
     pair — the TCP shape is deliberately payload-blind, with no
     L7 parsing.

DEFERRED-WIRING POSTURE: until the production composition root
supplies the callback service and its NSK validator, every request
to this endpoint returns 501 with `code:
access_session_not_provisioned` so log scrapers can alert on the
deferred-wiring state.


### Example


```python
import plexsphere
from plexsphere.models.session_activity_request import SessionActivityRequest
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
    api_instance = plexsphere.AccessApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Node identifier (UUIDv7) — the callback scope.
    session_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Session identifier (UUIDv7) the activity belongs to.
    authorization = 'authorization_example' # str | `Bearer <NSK plaintext>` — the per-Node Node Secret Key issued at registration time. The NSK is bound to the Node addressed by the path `id`; a credential belonging to a different Node surfaces as 403 `nsk_node_mismatch`. 
    session_activity_request = {"ssh":{"command":"journalctl -u plexd","exit_code":0,"started_at":"2026-05-02T10:06:00Z","completed_at":"2026-05-02T10:06:02Z"}} # SessionActivityRequest | 

    try:
        # Report per-kind session activity from a Node.
        api_instance.post_node_session_callback(id, session_id, authorization, session_activity_request)
    except Exception as e:
        print("Exception when calling AccessApi->post_node_session_callback: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Node identifier (UUIDv7) — the callback scope. | 
 **session_id** | **UUID**| Session identifier (UUIDv7) the activity belongs to. | 
 **authorization** | **str**| &#x60;Bearer &lt;NSK plaintext&gt;&#x60; — the per-Node Node Secret Key issued at registration time. The NSK is bound to the Node addressed by the path &#x60;id&#x60;; a credential belonging to a different Node surfaces as 403 &#x60;nsk_node_mismatch&#x60;.  | 
 **session_activity_request** | [**SessionActivityRequest**](SessionActivityRequest.md)|  | 

### Return type

void (empty response body)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**204** | Activity recorded. No body. |  -  |
**400** | Callback body failed structural validation — an invalid JSON envelope or an activity variant that does not match the session&#39;s kind.  |  -  |
**401** | NSK in the &#x60;Authorization: Bearer&#x60; header is missing, malformed, or has been revoked. The Problem body&#39;s &#x60;code&#x60; field is &#x60;nsk_revoked&#x60; so log scrapers can distinguish credential revocation from generic auth failures.  |  -  |
**403** | The NSK authenticates successfully but belongs to a different Node than the path &#x60;id&#x60;. The PermissionDenied body&#39;s &#x60;code&#x60; field is &#x60;nsk_node_mismatch&#x60;.  |  -  |
**404** | No Session resolves for the addressed &#x60;(id, session_id)&#x60; pair. Body is a &#x60;Problem&#x60; with &#x60;code: session_not_found&#x60;.  |  -  |
**409** | The Session is no longer live. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;session_already_revoked&#x60;, &#x60;session_expired&#x60; }.  |  -  |
**501** | The session-callback surface is not yet provisioned in this build. The Problem body&#39;s &#x60;code&#x60; field is &#x60;access_session_not_provisioned&#x60;.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **revoke_session**
> revoke_session(project_id, session_id, revoke_session_request)

Instantly revoke a live mediated session.

Revokes the Session identified by `{session_id}` within the owning
Project. The handler runs the revoke ReBAC check BEFORE any write,
then writes the revocation, appends the session identifier to the
revocation denylist, and fans a revocation event out over the
Signed Event Bus so the target plexd tears the listener down — all
in one transaction.

Re-revoking an already-revoked Session is idempotent: it returns
`204` and writes no additional rows.


### Example

* Bearer (JWT) Authentication (operatorBearer):

```python
import plexsphere
from plexsphere.models.revoke_session_request import RevokeSessionRequest
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
    api_instance = plexsphere.AccessApi(api_client)
    project_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Project. The Access Orchestrator issue, list, read, and revoke operations are scoped per Project, so every operator-facing endpoint requires the Project identifier in the path. 
    session_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Session identifier (UUIDv7). Bound on `/v1/projects/{project_id}/sessions/{session_id}`, its `:revoke` sub-resource, and its `/attach` WebSocket gateway for the single-Session read, revoke, and attach. 
    revoke_session_request = {"revoke_reason":"operator_request"} # RevokeSessionRequest | 

    try:
        # Instantly revoke a live mediated session.
        api_instance.revoke_session(project_id, session_id, revoke_session_request)
    except Exception as e:
        print("Exception when calling AccessApi->revoke_session: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **project_id** | **UUID**| Owning Project. The Access Orchestrator issue, list, read, and revoke operations are scoped per Project, so every operator-facing endpoint requires the Project identifier in the path.  | 
 **session_id** | **UUID**| Session identifier (UUIDv7). Bound on &#x60;/v1/projects/{project_id}/sessions/{session_id}&#x60;, its &#x60;:revoke&#x60; sub-resource, and its &#x60;/attach&#x60; WebSocket gateway for the single-Session read, revoke, and attach.  | 
 **revoke_session_request** | [**RevokeSessionRequest**](RevokeSessionRequest.md)|  | 

### Return type

void (empty response body)

### Authorization

[operatorBearer](../README.md#operatorBearer)

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**204** | Session revoked (or already revoked — the operation is idempotent). No body.  |  -  |
**400** | Malformed id or body. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;invalid_session_id&#x60;, &#x60;invalid_body&#x60; }.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to revoke this Session. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Session not found. Body is a &#x60;Problem&#x60; with &#x60;code: session_not_found&#x60;.  |  -  |
**501** | The access-orchestrator surface is not yet provisioned in this build. The Problem body&#39;s &#x60;code&#x60; field is &#x60;access_session_not_provisioned&#x60;.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

