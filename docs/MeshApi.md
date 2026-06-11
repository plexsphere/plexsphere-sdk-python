# plexsphere.MeshApi

All URIs are relative to *http://localhost*

Method | HTTP request | Description
------------- | ------------- | -------------
[**delete_node_state_report**](MeshApi.md#delete_node_state_report) | **DELETE** /v1/nodes/{id}/state/reports/{key} | Delete an upstream node-state report entry for a Node.
[**fetch_node_secret**](MeshApi.md#fetch_node_secret) | **GET** /v1/nodes/{id}/secrets/{name} | Fetch a Secret Store entry rewrapped under the calling Node&#39;s NSK.
[**get_domain_mesh_topology**](MeshApi.md#get_domain_mesh_topology) | **GET** /v1/domains/{domainId}/mesh/topology | Return the mesh topology for a Domain.
[**get_node_events**](MeshApi.md#get_node_events) | **GET** /v1/nodes/{id}/events | Stream signed envelope events for a Node over SSE.
[**get_node_keys_rotate_preview**](MeshApi.md#get_node_keys_rotate_preview) | **GET** /v1/nodes/{id}/keys/rotate/preview | Preview the fleet impact of rotating a Node&#39;s mesh key.
[**get_node_reachability**](MeshApi.md#get_node_reachability) | **GET** /v1/nodes/{id}/reachability | Read the reachability projection for a Node.
[**get_node_state**](MeshApi.md#get_node_state) | **GET** /v1/nodes/{id}/state | Reconciliation pull for a Node — return the canonical NodeStateSnapshot.
[**post_keys_rotate**](MeshApi.md#post_keys_rotate) | **POST** /v1/keys/rotate | Submit a freshly-generated Curve25519 key to complete a rotation.
[**post_node_audit**](MeshApi.md#post_node_audit) | **POST** /v1/nodes/{id}/audit | Ingest a batch of normalised audit events for a Node.
[**post_node_heartbeat**](MeshApi.md#post_node_heartbeat) | **POST** /v1/nodes/{id}/heartbeat | Record a Node liveness heartbeat and return reconcile/rotate hints.
[**post_node_integrity_violations**](MeshApi.md#post_node_integrity_violations) | **POST** /v1/nodes/{id}/integrity-violations | Ingest a batch of integrity-violation reports for a Node.
[**post_node_keys_rotate**](MeshApi.md#post_node_keys_rotate) | **POST** /v1/nodes/{id}/keys/rotate | Trigger a mesh-key rotation for a Node.
[**post_node_logs**](MeshApi.md#post_node_logs) | **POST** /v1/nodes/{id}/logs | Ingest a batch of structured log lines for a Node.
[**post_node_metrics**](MeshApi.md#post_node_metrics) | **POST** /v1/nodes/{id}/metrics | Ingest a batch of metric samples for a Node.
[**put_node_capabilities**](MeshApi.md#put_node_capabilities) | **PUT** /v1/nodes/{id}/capabilities | Record the per-Node capability manifest snapshot.
[**put_node_endpoint**](MeshApi.md#put_node_endpoint) | **PUT** /v1/nodes/{id}/endpoint | Record a NAT-observed Peer endpoint observation.
[**put_node_state_report**](MeshApi.md#put_node_state_report) | **PUT** /v1/nodes/{id}/state/reports/{key} | Record an upstream node-state report entry for a Node.


# **delete_node_state_report**
> delete_node_state_report(id, key, authorization)

Delete an upstream node-state report entry for a Node.

Deletes the upstream node-state report entry the addressed Node
recorded under the addressed key. Deleting a report emits NO
`node_state_updated` event. A delete for a key the Node carries
no report under is answered with 404.

The caller authenticates with its NSK in the `Authorization:
Bearer` header; the handler asserts the resolved NSK Node matches
the path `id` so a Node may only delete its own reports.

DEFERRED-WIRING POSTURE: when the node-state service is not wired
into this build every request returns 501 with
`code: reports_not_provisioned`.


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
    api_instance = plexsphere.MeshApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Node identifier (UUIDv7) — the report-delete scope.
    key = 'key_example' # str | Report entry key. Lower-case, starts with a letter, and is limited to letters, digits, dot, hyphen, and underscore (1 to 128 characters). 
    authorization = 'authorization_example' # str | `Bearer <NSK plaintext>` — the per-Node Node Secret Key. Only a Node proving possession of its NSK may delete a report; a missing, malformed, or revoked credential surfaces as 401. 

    try:
        # Delete an upstream node-state report entry for a Node.
        api_instance.delete_node_state_report(id, key, authorization)
    except Exception as e:
        print("Exception when calling MeshApi->delete_node_state_report: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Node identifier (UUIDv7) — the report-delete scope. | 
 **key** | **str**| Report entry key. Lower-case, starts with a letter, and is limited to letters, digits, dot, hyphen, and underscore (1 to 128 characters).  | 
 **authorization** | **str**| &#x60;Bearer &lt;NSK plaintext&gt;&#x60; — the per-Node Node Secret Key. Only a Node proving possession of its NSK may delete a report; a missing, malformed, or revoked credential surfaces as 401.  | 

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
**204** | The report entry was deleted. No response body.  |  -  |
**401** | NSK in the &#x60;Authorization: Bearer&#x60; header is missing, malformed, or has been revoked. The Problem body&#39;s &#x60;code&#x60; field is &#x60;unauthorized&#x60;.  |  -  |
**403** | The NSK resolves to a different Node than the path &#x60;id&#x60; — a Node may only delete its own reports. The PermissionDenied body&#39;s &#x60;code&#x60; field is &#x60;node_id_mismatch&#x60; so a leaked NSK cannot be replayed against a sibling Node.  |  -  |
**404** | No report entry exists under the addressed key for the addressed Node (or the Node itself does not resolve). The Problem body&#39;s &#x60;code&#x60; field is &#x60;report_not_found&#x60;.  |  -  |
**501** | The node-state service is not yet provisioned in this build. The Problem body&#39;s &#x60;code&#x60; field is &#x60;reports_not_provisioned&#x60;.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **fetch_node_secret**
> bytes fetch_node_secret(id, name, authorization, version=version)

Fetch a Secret Store entry rewrapped under the calling Node's NSK.

Returns the named Secret Store entry for the addressed Node as an
AES-256-GCM ciphertext envelope rewrapped under the calling
Node's Node Secret Key (NSK). The Secret Store is the platform's
only meet-and-rewrap point between the OpenBao backend and the
per-Node NSK: the handler reads the OpenBao plaintext, recovers
the NSK, and rewraps the payload in-process so that ONLY
NSK-wrapped ciphertext ever crosses the wire. The transient
OpenBao plaintext and the recovered NSK are zeroed on every exit
path; no plaintext is ever persisted, logged, cached, or placed
on the event bus.

The 200 body is the raw envelope `<12-byte nonce> ||
<ciphertext + 16-byte GCM tag>`, served as
`application/octet-stream`. The caller recovers the seeded
plaintext byte-for-byte with `AES-256-GCM-Open` under its NSK.
The response carries the served version in
`X-Plexsphere-Secret-Version`, the NSK key id used for the wrap
in `X-Plexsphere-Secret-KID` (so plexd can pick the right NSK
during a rotation overlap), and `Cache-Control: no-store` so no
intermediary ever retains the ciphertext.

The handler:

  1. Authenticates the caller against the Node Secret Key (NSK)
     plaintext supplied in the `Authorization: Bearer` header,
     refusing missing or revoked credentials with 401. Only a
     Node proving possession of its NSK can reach the rewrap
     pipeline.
  2. Resolves the addressed secret metadata, applies the
     per-Node and per-Domain rate limits, and runs the
     `node-agent` ReBAC visibility check on the addressed Node.
     An authenticated caller without the relation receives 403
     `PermissionDenied`; the denial is recorded with
     `insufficient_relation`.
  3. Reads the entry from the OpenBao backend — the current
     version when `version` is omitted, or the requested version
     when present — reconciling the served version against the
     metadata; the OpenBao-returned version wins for the header.
  4. Rewraps the payload under the recovered NSK with a fresh
     12-byte nonce and emits exactly one granted audit row. A
     backend read that fails AFTER a granted authorization still
     records the granted audit row — operational outcomes are
     carried on the fetch-duration metric, never on the audit
     chain.

The response body is capped at 1 MiB of ciphertext.

DEFERRED-WIRING POSTURE: when the Secret Store backend is not
configured (empty backend DSN) the production composition root
leaves this surface disabled and every request returns 501 with
`code: secrets_not_provisioned` so log scrapers can alert on the
deferred-wiring state.


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
    api_instance = plexsphere.MeshApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Node identifier (UUIDv7) — the secret-fetch scope.
    name = 'name_example' # str | Secret name within the owning Project. Lower-case, starts with a letter, and is limited to letters, digits, hyphen, and underscore (1 to 63 characters). 
    authorization = 'authorization_example' # str | `Bearer <NSK plaintext>` — the per-Node Node Secret Key issued at registration time. Only a Node proving possession of its NSK may fetch a secret; a missing, malformed, or revoked credential surfaces as 401. 
    version = 56 # int | Optional OpenBao KV v2 version to fetch. When omitted the current version is served. The version actually served is always reported in the `X-Plexsphere-Secret-Version` response header.  (optional)

    try:
        # Fetch a Secret Store entry rewrapped under the calling Node's NSK.
        api_response = api_instance.fetch_node_secret(id, name, authorization, version=version)
        print("The response of MeshApi->fetch_node_secret:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling MeshApi->fetch_node_secret: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Node identifier (UUIDv7) — the secret-fetch scope. | 
 **name** | **str**| Secret name within the owning Project. Lower-case, starts with a letter, and is limited to letters, digits, hyphen, and underscore (1 to 63 characters).  | 
 **authorization** | **str**| &#x60;Bearer &lt;NSK plaintext&gt;&#x60; — the per-Node Node Secret Key issued at registration time. Only a Node proving possession of its NSK may fetch a secret; a missing, malformed, or revoked credential surfaces as 401.  | 
 **version** | **int**| Optional OpenBao KV v2 version to fetch. When omitted the current version is served. The version actually served is always reported in the &#x60;X-Plexsphere-Secret-Version&#x60; response header.  | [optional] 

### Return type

**bytes**

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/octet-stream, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | The rewrapped secret envelope &#x60;&lt;12-byte nonce&gt; || &lt;ciphertext + 16-byte GCM tag&gt;&#x60;. &#x60;AES-256-GCM-Open&#x60; under the calling Node&#39;s NSK recovers the seeded cleartext byte-for-byte. The body is opaque ciphertext and is never logged.  |  * X-Plexsphere-Secret-Version - The version actually served — the version OpenBao returned, which wins over the metadata current version when KV v2 promoted a version server-side.  <br>  * X-Plexsphere-Secret-KID - The NSK key id used to wrap the envelope. Lets plexd pick the correct NSK to unwrap with during a key-rotation overlap window.  <br>  * Cache-Control - Always &#x60;no-store&#x60; — the ciphertext envelope MUST NOT be retained by any intermediary or client cache.  <br>  |
**401** | NSK in the &#x60;Authorization: Bearer&#x60; header is missing, malformed, or has been revoked. The Problem body&#39;s &#x60;code&#x60; field is &#x60;unauthorized&#x60;.  |  -  |
**403** | The NSK authenticates successfully but the calling Node lacks the &#x60;node-agent&#x60; ReBAC relation that gates visibility of the addressed secret. The PermissionDenied body carries the ReBAC denial &#x60;reason&#x60; and the &#x60;correlation_id&#x60; that pairs with the audit entry; surfaced only after authentication so the endpoint cannot be used as a secret-name oracle.  |  -  |
**404** | No secret resolves for the addressed Node and name. The Problem body&#39;s &#x60;code&#x60; field disambiguates the cause:    - &#x60;secret_not_found&#x60; — no metadata row exists for the     addressed Node and name.   - &#x60;secret_version_not_found&#x60; — the requested &#x60;version&#x60; does     not exist at the backend.  No audit row is emitted for a non-existent secret — nothing was authorized.  |  -  |
**429** | The per-Node or per-Domain secret-fetch rate-limit bucket is exhausted. The Problem body&#39;s &#x60;code&#x60; field is one of &#x60;per_node_rate_limited&#x60; or &#x60;per_domain_rate_limited&#x60;, and the &#x60;Retry-After&#x60; response header carries the number of seconds the caller should wait before retrying.  |  * Retry-After - Seconds the caller should wait before retrying the batch.  <br>  |
**503** | The OpenBao backend is sealed or unavailable, so an authorized read could not be completed. The Problem body&#39;s &#x60;code&#x60; field is &#x60;openbao_unavailable&#x60;. A backend-unavailable read that follows a granted authorization still emits one granted audit row.  |  -  |
**501** | The Secret Store surface is not yet provisioned in this build — the backend DSN is unset, so the rewrap pipeline is disabled. The Problem body&#39;s &#x60;code&#x60; field is &#x60;secrets_not_provisioned&#x60; so log scrapers can alert on the deferred-wiring state. Resolves once the Secret Store backend is configured and the fetch service and its ports are wired into the v1 handler dependency cone.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **get_domain_mesh_topology**
> MeshTopology get_domain_mesh_topology(domain_id)

Return the mesh topology for a Domain.

Returns the read-side projection of the mesh fabric that backs
the dashboard mesh-map view and the `plexctl mesh topology`
CLI: every live Peer Node anchored to the Domain together with
the directed edges between them. The handler:

  1. Authenticates the caller and rejects requests without a
     resolved Principal with 401.
  2. Checks the `domain-view` ReBAC relation on the addressed
     Domain BEFORE any existence check so the endpoint cannot
     be used as a Domain-id oracle.
  3. Resolves the per-Domain peer graph from the SSE peer-
     delta projection — `nodes` carries each anchored Node's
     mesh-IP and reachability, `edges` carries each pairwise
     relationship with its `mode` (`direct` or `relayed`), the
     handshake age, and the bridge Node currently serving as
     the relay fallback when one is assigned.

The payload mirrors the SSE peer-graph events one-for-one so a
renderer that consumes the live stream and a renderer that
polls this pull converge on byte-identical state.

DEFERRED-WIRING POSTURE: until the production composition root
supplies the mesh-topology projection and the ReBAC
RelationChecker the handler depends on, every request to this
endpoint returns 501 with
`code: mesh_topology_not_provisioned` so log scrapers can
alert on the deferred-wiring state.


### Example


```python
import plexsphere
from plexsphere.models.mesh_topology import MeshTopology
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
    api_instance = plexsphere.MeshApi(api_client)
    domain_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Domain identifier (UUIDv7) — the topology scope.

    try:
        # Return the mesh topology for a Domain.
        api_response = api_instance.get_domain_mesh_topology(domain_id)
        print("The response of MeshApi->get_domain_mesh_topology:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling MeshApi->get_domain_mesh_topology: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **domain_id** | **UUID**| Domain identifier (UUIDv7) — the topology scope. | 

### Return type

[**MeshTopology**](MeshTopology.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Mesh topology snapshot for the addressed Domain.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the &#x60;domain-view&#x60; ReBAC relation on the addressed Domain. Surfaced before any existence check so the endpoint cannot be used as a Domain-id oracle.  |  -  |
**404** | Domain id does not match any Domain. Surfaced only after the authorisation gate has passed so the endpoint cannot be used as a Domain-id oracle. The Problem body&#39;s &#x60;code&#x60; field is &#x60;domain_not_found&#x60;.  |  -  |
**500** | Internal server error. A projection-read failure or an authorization-check fault surfaces here.  |  -  |
**501** | The mesh-topology read surface is not yet provisioned in this build. The Problem body&#39;s &#x60;code&#x60; field is &#x60;mesh_topology_not_provisioned&#x60; so log scrapers can alert on the deferred-wiring state. Resolves once the topology projection and the ReBAC RelationChecker are wired into the v1 handler dependency cone.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **get_node_events**
> str get_node_events(id, last_event_id=last_event_id)

Stream signed envelope events for a Node over SSE.

Opens a Server-Sent Events stream of canonical signed
`Envelope` records scoped to the addressed Node. Each `data:` frame is the JSON wire body
produced by `internal/signing/envelope.CanonicalBytes`. The
server runs ed25519 verification against the per-Domain
signing public key before emitting the frame; consumers
SHOULD verify the trailing `signature:` field again as a
defence-in-depth measure.

The `id:` field on every event frame is the JetStream stream
sequence number for that envelope. Clients resume by re-
connecting with `Last-Event-ID: <numeric>` set to the last
sequence they durably processed; the server replays from the
next sequence (`> last`). When the header is absent or empty
the stream tails from now — historical events are NOT
backfilled.

The server emits a 25-second SSE comment-frame keep-alive
(`:keep-alive\n\n`) so idle proxies do not collapse the
connection. The `X-Plexsphere-API-Version` response header
carries the contract version the stream conforms to;
`Cache-Control: no-cache` opts the response out of any
intermediary caching layer.

DEFERRED-WIRING POSTURE: the production
composition root does not yet supply the SignatureVerifier,
RelationChecker, or NodeRepo ports the handler depends on.
Until those ports are wired, every request to this endpoint
returns 501 with `code: signed_event_bus_not_provisioned`.
See docs/architecture/mesh-event-bus-roadmap.md for the deferred work tracking.


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
    api_instance = plexsphere.MeshApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Node identifier (UUIDv7) — the SSE stream scope.
    last_event_id = 'last_event_id_example' # str | SSE replay cursor. When present and parseable as a non-negative integer, the server resumes streaming from the envelope whose JetStream sequence is strictly greater than the supplied value. When absent or empty the stream tails from now.  (optional)

    try:
        # Stream signed envelope events for a Node over SSE.
        api_response = api_instance.get_node_events(id, last_event_id=last_event_id)
        print("The response of MeshApi->get_node_events:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling MeshApi->get_node_events: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Node identifier (UUIDv7) — the SSE stream scope. | 
 **last_event_id** | **str**| SSE replay cursor. When present and parseable as a non-negative integer, the server resumes streaming from the envelope whose JetStream sequence is strictly greater than the supplied value. When absent or empty the stream tails from now.  | [optional] 

### Return type

**str**

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: text/event-stream, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | SSE stream of signed envelope events. The body is an unbounded &#x60;text/event-stream&#x60; — each event frame carries an &#x60;id:&#x60; (JetStream sequence), an &#x60;event:&#x60; discriminator, and a &#x60;data:&#x60; line whose payload is the canonical signed envelope JSON. The currently emitted &#x60;event:&#x60; values are &#x60;node_state_updated&#x60;, &#x60;policy_updated&#x60;, and &#x60;bridge_config_updated&#x60;; &#x60;policy_updated&#x60; carries the per-(Node, Policy) compiled-ruleset projection described by &#x60;PolicyUpdatedPayload&#x60;, while &#x60;bridge_config_updated&#x60; carries the per-(Node, bridge Resource) delivered-config projection described by &#x60;BridgeConfigUpdatedPayload&#x60;. Additional discriminators land as the README&#39;s 14-type taxonomy ships. Comment frames every 25 seconds keep idle proxies from collapsing the connection.  |  * X-Plexsphere-API-Version - Contract version this SSE stream conforms to. Lets consumers branch on schema evolution without parsing the OpenAPI document at runtime.  <br>  * Cache-Control - Always &#x60;no-store&#x60; — the ciphertext envelope MUST NOT be retained by any intermediary or client cache.  <br>  |
**400** | &#x60;Last-Event-ID&#x60; header was present but unparseable — non-numeric, negative, or otherwise outside the integer domain. The stream is NOT opened.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the node-agent ReBAC relation on the addressed Node and may not subscribe to its event stream .  |  -  |
**404** | Node id not found. |  -  |
**501** | The Signed Event Bus surface is not yet provisioned in this build. The Problem body&#39;s &#x60;code&#x60; field is &#x60;signed_event_bus_not_provisioned&#x60; so log scrapers can alert on the deferred-wiring state. Resolves once SignatureVerifier, RelationChecker, and NodeRepo are wired into the v1 handler dependency cone — see docs/architecture/mesh-event-bus-roadmap.md.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **get_node_keys_rotate_preview**
> RotationImpactPreview get_node_keys_rotate_preview(id)

Preview the fleet impact of rotating a Node's mesh key.

Returns a non-mutating dry-run of the fleet impact a manual
mesh-key rotation against the addressed Node would have. Backs
the Phase-2 dashboard "manual key-rotation with impact preview"
view and the `plexctl key rotate --preview` CLI. The handler:

  1. Authenticates the caller and rejects requests without a
     resolved Principal with 401.
  2. Checks the same operator-action ReBAC relation
     `node-operate` on `node:<id>` that the mutating
     `POST /v1/nodes/{id}/keys/rotate` checks, BEFORE any
     existence check, so an unauthorised caller cannot probe
     Node existence via the 404/403 timing differential. A
     caller lacking the relation receives 403
     `insufficient_relation` and an audit row with outcome
     `insufficient_relation`.
  3. Resolves the per-Node read-side projection — the live
     Peer the rotation would target, every peer edge whose
     PSK would be re-issued, and the in-flight rotation row
     (if any). NO `peer_key_rotation` row is created, NO
     `rotate_keys` outbox event is appended, and NO audit row
     beyond the read-side ReBAC decision is emitted.

The 200 response payload mirrors the mutating endpoint's view
of the world one-for-one — `peer_id` matches what the live
`POST` would return, `already_pending` flips on a pre-existing
in-flight rotation the same way — so a renderer that consumes
both surfaces can drive the dashboard confirmation dialog from
a single mapping.

DEFERRED-WIRING POSTURE: until the production composition root
supplies the read-side preview projection and the ReBAC
RelationChecker the handler depends on, every request to this
endpoint returns 501 with
`code: node_keys_rotate_preview_not_provisioned` so log
scrapers can alert on the deferred-wiring state.


### Example


```python
import plexsphere
from plexsphere.models.rotation_impact_preview import RotationImpactPreview
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
    api_instance = plexsphere.MeshApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Node identifier (UUIDv7) — the rotation scope.

    try:
        # Preview the fleet impact of rotating a Node's mesh key.
        api_response = api_instance.get_node_keys_rotate_preview(id)
        print("The response of MeshApi->get_node_keys_rotate_preview:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling MeshApi->get_node_keys_rotate_preview: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Node identifier (UUIDv7) — the rotation scope. | 

### Return type

[**RotationImpactPreview**](RotationImpactPreview.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Impact preview for the requested rotation. Carries the target Peer, the affected-edge listing, the &#x60;already_pending&#x60; flag, and an estimated wall-clock duration so the renderer can size its progress indicator.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the &#x60;node-operate&#x60; ReBAC relation on the addressed Node and may not preview a key rotation. The PermissionDenied body&#39;s &#x60;reason&#x60; field is &#x60;insufficient_relation&#x60;. Surfaced before any existence check so the endpoint cannot be used as a Node-id oracle.  |  -  |
**404** | Node id does not match any Node in this Domain. Surfaced only after the authorisation gate has passed so the endpoint cannot be used as a Node-id oracle. The Problem body&#39;s &#x60;code&#x60; field is &#x60;node_not_found&#x60;.  |  -  |
**500** | Internal server error. A projection-read failure or an authorization-check fault surfaces here.  |  -  |
**501** | The key-rotation impact-preview surface is not yet provisioned in this build. The Problem body&#39;s &#x60;code&#x60; field is &#x60;node_keys_rotate_preview_not_provisioned&#x60; so log scrapers can alert on the deferred-wiring state. Resolves once the preview projection and the ReBAC RelationChecker are wired into the v1 handler dependency cone.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **get_node_reachability**
> Reachability get_node_reachability(id)

Read the reachability projection for a Node.

Returns the latest `Reachability` projection for the addressed
Node — the per-Node state-machine the heartbeat handler
advances on every accepted heartbeat. The projection is the authoritative health view the
operator UI and reconciliation surface consume; it transitions
`healthy` → `stale` after 90s without a heartbeat and `stale`
→ `unreachable` after 300s, with `changed_at` tracking the
most recent transition.

The endpoint reuses the `node-agent` ReBAC relation already
guarding `GET /v1/nodes/{id}/state` and
`GET /v1/nodes/{id}/events`, so any caller authorised to issue
a reconciliation pull is also authorised to read the
reachability projection. Unauthenticated callers receive 401;
authenticated callers without the relation receive 403; an
unknown Node id surfaces as 404 only after the authorisation
gate passes so the endpoint cannot be used as a Node-id oracle
.

DEFERRED-WIRING POSTURE: until the production
composition root supplies the ReachabilityRepo, RelationChecker,
and NodeRepo ports the handler depends on, every request to
this endpoint returns 501 with `code:
reachability_not_provisioned` so log scrapers can alert on the
deferred-wiring state. See docs/architecture/mesh-event-bus-roadmap.md for the
deferred work tracking.


### Example


```python
import plexsphere
from plexsphere.models.reachability import Reachability
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
    api_instance = plexsphere.MeshApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Node identifier (UUIDv7) — the reachability scope.

    try:
        # Read the reachability projection for a Node.
        api_response = api_instance.get_node_reachability(id)
        print("The response of MeshApi->get_node_reachability:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling MeshApi->get_node_reachability: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Node identifier (UUIDv7) — the reachability scope. | 

### Return type

[**Reachability**](Reachability.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Latest &#x60;Reachability&#x60; projection for the addressed Node. &#x60;last_heartbeat_at&#x60; is &#x60;null&#x60; until the first heartbeat is accepted; &#x60;changed_at&#x60; is always present and tracks the most recent state transition.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the &#x60;node-agent&#x60; ReBAC relation on the addressed Node and may not read the reachability projection. The Problem body&#39;s &#x60;code&#x60; field is &#x60;insufficient_relation&#x60; .  |  -  |
**404** | Node id not found. Surfaced only after the authorisation gate has passed so the endpoint cannot be used as a Node-id oracle. The Problem body&#39;s &#x60;code&#x60; field is &#x60;node_not_found&#x60;.  |  -  |
**501** | The reachability surface is not yet provisioned in this build. The Problem body&#39;s &#x60;code&#x60; field is &#x60;reachability_not_provisioned&#x60; so log scrapers can alert on the deferred-wiring state. Resolves once ReachabilityRepo, RelationChecker, and NodeRepo are wired into the v1 handler dependency cone (see docs/architecture/mesh-event-bus-roadmap.md).  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **get_node_state**
> NodeStateSnapshot get_node_state(id)

Reconciliation pull for a Node — return the canonical NodeStateSnapshot.

Returns the canonical `NodeStateSnapshot` for the addressed
Node. The snapshot is the authoritative
cold-start view that plexd consumes when it first comes up,
when its SSE connection has been disconnected for longer than
the replay window, or when an out-of-band request arrives to
re-derive the desired state. The four wire blocks — `peers`,
`policy`, `bridge`, and `state`/`reports` — are always present
so plexd's reconcile loop can diff by field presence rather
than absence; later stories populate the
currently-empty blocks without changing the wire shape
.

The peer projection is a single SQL round-trip ordered by
`node_id ASC` so two consecutive pulls against the same
ledger snapshot are byte-equal — plexd's reconcile loop
reduces redundant rewrites by hashing the response.

The endpoint reuses the `node-agent` ReBAC relation already
guarding `GET /v1/nodes/{id}/events`, so any caller authorised to subscribe to a
Node's SSE event stream is also authorised to issue a
reconciliation pull. Unauthenticated callers receive 401;
authenticated callers without the relation receive 403; an
unknown Node id surfaces as 404 only after the authorisation
gate passes so the endpoint cannot be used as a Node-id
oracle.

DEFERRED-WIRING POSTURE: until the production
composition root supplies the `SnapshotProvider`,
`RelationChecker`, and `NodeRepo` ports the handler depends
on, every request to this endpoint returns 501 with `code:
signed_event_bus_not_provisioned` so log scrapers can alert
on the deferred-wiring state. See docs/architecture/mesh-event-bus-roadmap.md for
the deferred work tracking.


### Example


```python
import plexsphere
from plexsphere.models.node_state_snapshot import NodeStateSnapshot
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
    api_instance = plexsphere.MeshApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Node identifier (UUIDv7) — the snapshot scope.

    try:
        # Reconciliation pull for a Node — return the canonical NodeStateSnapshot.
        api_response = api_instance.get_node_state(id)
        print("The response of MeshApi->get_node_state:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling MeshApi->get_node_state: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Node identifier (UUIDv7) — the snapshot scope. | 

### Return type

[**NodeStateSnapshot**](NodeStateSnapshot.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Canonical &#x60;NodeStateSnapshot&#x60; for the addressed Node. The &#x60;peers&#x60; array carries one entry per other Node in the addressed Node&#39;s Domain; the addressed Node itself is excluded so plexd does not program a self-peer. The remaining blocks (&#x60;policy&#x60;, &#x60;bridge&#x60;, &#x60;state&#x60;, &#x60;reports&#x60;) are present-but-empty placeholders that later stories populate without changing the wire shape.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the &#x60;node-agent&#x60; ReBAC relation on the addressed Node and may not issue a reconciliation pull .  |  -  |
**404** | Node id not found. Surfaced only after the authorisation gate has passed so the endpoint cannot be used as a Node-id oracle.  |  -  |
**501** | The reconciliation-pull surface is not yet provisioned in this build. The Problem body&#39;s &#x60;code&#x60; field is &#x60;signed_event_bus_not_provisioned&#x60; so log scrapers can alert on the deferred-wiring state — the same code used by the SSE peer endpoint, since both surfaces share the same fail-closed posture and unblock together. Resolves once &#x60;SnapshotProvider&#x60;, &#x60;RelationChecker&#x60;, and &#x60;NodeRepo&#x60; are wired into the v1 handler dependency cone (see docs/architecture/mesh-event-bus-roadmap.md).  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **post_keys_rotate**
> KeysRotateResponse post_keys_rotate(keys_rotate_request)

Submit a freshly-generated Curve25519 key to complete a rotation.

Completes a pending mesh-key rotation for the calling Node.
After an operator triggers a rotation via
`POST /v1/nodes/{id}/keys/rotate` — or the heartbeat response
sets `rotate_keys: true` — plexd generates a fresh Curve25519
keypair and submits the new public key here. The handler:

  1. Authenticates the caller against the Node Secret Key
     (NSK) plaintext supplied in the `Authorization: Bearer`
     header. The route carries NO path `{id}` segment — the
     rotating Node is identified solely by the NSK envelope it
     presents — so a missing or unresolved NSK surfaces as
     401.
  2. Caps the request body at 4 KiB; oversize bodies receive
     413 `keys_rotate_body_too_large` without ever touching
     the JSON decoder.
  3. Validates the submitted `new_public_key` — it MUST decode
     to exactly 32 bytes and MUST NOT be the all-zero
     degenerate value — BEFORE any database read (otherwise
     422 `keys_rotate_public_key_invalid`).
  4. Resolves the caller's live Peer row; a Node with no live
     Peer surfaces as 404 `keys_rotate_peer_not_found`.
  5. Hands the new key to the rotation completer, which — in
     one transaction — updates `nodes.public_key`, retires the
     old pairwise PSK, wraps and inserts a fresh PSK, appends
     a `peer_key_rotated` outbox event, and flips the pending
     `peer_key_rotation` row to `completed`.

The 200 response carries the completed rotation's id plus the
`(kid, wrap_key_version)` reference of the re-issued PSK — it
never carries PSK plaintext or ciphertext. An idempotent retry
with the same key against an already-completed rotation
returns the prior receipt and emits no second event.

DEFERRED-WIRING POSTURE: until the production composition root
supplies the RotationCompleter, NSKResolver, NodeRepo, and
PeerLookup ports the handler depends on, every request to this
endpoint returns 501 with `code: keys_rotate_not_provisioned`
so log scrapers can alert on the deferred-wiring state.


### Example


```python
import plexsphere
from plexsphere.models.keys_rotate_request import KeysRotateRequest
from plexsphere.models.keys_rotate_response import KeysRotateResponse
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
    api_instance = plexsphere.MeshApi(api_client)
    keys_rotate_request = {"new_public_key":"AAECAwQFBgcICQoLDA0ODxAREhMUFRYXGBkaGxwdHh8="} # KeysRotateRequest | 

    try:
        # Submit a freshly-generated Curve25519 key to complete a rotation.
        api_response = api_instance.post_keys_rotate(keys_rotate_request)
        print("The response of MeshApi->post_keys_rotate:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling MeshApi->post_keys_rotate: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **keys_rotate_request** | [**KeysRotateRequest**](KeysRotateRequest.md)|  | 

### Return type

[**KeysRotateResponse**](KeysRotateResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Rotation completed. &#x60;nodes.public_key&#x60; has been updated, the old PSK retired, a fresh wrapped PSK inserted, and the pending &#x60;peer_key_rotation&#x60; row flipped to &#x60;completed&#x60; — all in one transaction. The response carries the rotation id and the re-issued PSK&#39;s &#x60;(kid, wrap_key_version)&#x60; reference.  |  -  |
**400** | Request body could not be decoded as a KeysRotateRequest envelope (invalid JSON, unknown field). The Problem body&#39;s &#x60;code&#x60; field is &#x60;malformed_keys_rotate_request&#x60;.  |  -  |
**401** | NSK in the &#x60;Authorization: Bearer&#x60; header is missing, malformed, or has been revoked, so the rotating Node could not be resolved. The Problem body&#39;s &#x60;code&#x60; field is &#x60;unauthorized&#x60;.  |  -  |
**403** | The ReBAC middleware could not authorize the calling principal for the key-rotation surface — the authenticated Node does not map to a Subject permitted to complete a rotation. The PermissionDenied body carries the ReBAC denial &#x60;reason&#x60; and the &#x60;correlation_id&#x60; that pairs with the audit entry.  |  -  |
**404** | No live Peer row resolves for the authenticated Node — the Node was drained or has never been bound to a Peer. The Problem body&#39;s &#x60;code&#x60; field is &#x60;keys_rotate_peer_not_found&#x60;.  |  -  |
**409** | No pending &#x60;peer_key_rotation&#x60; row exists for the authenticated Node — the submission has no rotation to complete. The Problem body&#39;s &#x60;code&#x60; field is &#x60;keys_rotate_no_pending_rotation&#x60;.  |  -  |
**413** | Request body exceeded the 4 KiB ceiling enforced by the handler. The Problem body&#39;s &#x60;code&#x60; field is &#x60;keys_rotate_body_too_large&#x60; so the handler can refuse a pathological payload without ever invoking the JSON decoder.  |  -  |
**422** | The submitted &#x60;new_public_key&#x60; failed a structural or invariant check. The Problem body&#39;s &#x60;code&#x60; field disambiguates the cause:    - &#x60;keys_rotate_public_key_invalid&#x60; — the key did not     decode to exactly 32 bytes, was the all-zero     degenerate value, or failed the rotation aggregate&#39;s     key invariants.   - &#x60;keys_rotate_public_key_unchanged&#x60; — the key is     byte-identical to the Node&#39;s current public key; a     rotation must install a genuinely new key.  |  -  |
**500** | Internal server error. A Peer-lookup failure or a rotation-completion persistence fault surfaces here with a non-revealing detail string; the rotation transaction rolls back atomically.  |  -  |
**501** | The key-rotation surface is not yet provisioned in this build. The Problem body&#39;s &#x60;code&#x60; field is &#x60;keys_rotate_not_provisioned&#x60; so log scrapers can alert on the deferred-wiring state. Resolves once the RotationCompleter, NSKResolver, NodeRepo, and PeerLookup ports are wired into the v1 handler dependency cone.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **post_node_audit**
> IngestReceipt post_node_audit(id, authorization, audit_event, x_plexsphere_sent_at=x_plexsphere_sent_at)

Ingest a batch of normalised audit events for a Node.

Accepts a gzip-compressed newline-delimited JSON
(`application/x-ndjson`) batch of `AuditEvent` records — one object
per line — pushed by plexd and hands the batch to the per-signal
ingest buffer (a JetStream subject keyed by the originating Node's
Domain), from which the platform routes to Grafana Loki with a
separate retention class and a distinct SIEM route. Every record is
tagged server-side with the Domain and Project of the originating
Node so dashboards and alerts scope through the platform label
model; the wire body never carries an Identity subject or email
(see the de-personalisation note below).

The handler applies the following gates in order; each maps to a
single stable `code` on an RFC 9457 Problem body so generated
clients can switch on `code` exhaustively:

  1. Authenticates the caller against the Node Secret Key (NSK)
     plaintext in the `Authorization: Bearer` header, refusing a
     missing, malformed, or revoked credential with 401.
  2. Asserts the NSK belongs to the Node addressed by the path
     `id`, refusing cross-Node use with 403 `node_id_mismatch` so
     a leaked NSK cannot be replayed against a sibling Node.
  3. Refuses a `Content-Encoding` other than `gzip` or `identity`
     with 415 `ingest_encoding_unsupported`.
  4. Requires the `X-Plexsphere-Sent-At` header to be present and
     an RFC 3339 timestamp; a missing or unparseable value is
     refused with 400 `ingest_sent_at_invalid`. The header drives
     the ingest-lag metric.
  5. Caps the compressed wire body; a body over the cap is refused
     with 413 `ingest_body_too_large` before the gzip reader runs.
  6. Applies the per-Node then per-Domain byte-weighted quota; an
     exhausted per-Node bucket surfaces as 429
     `per_node_rate_limited` and an exhausted per-Domain bucket as
     429 `capacity_exceeded`, both carrying a `Retry-After` header.
  7. Inflates the gzip stream under a decompression cap (a stream
     that fails to inflate is 400 `ingest_encoding_invalid`; an
     inflated batch over the cap is 413 `ingest_body_too_large`)
     and structurally validates each line (a malformed line or a
     `source` outside the closed set is 400
     `ingest_batch_malformed`; more than the per-batch record cap
     is 413 `ingest_batch_too_many_records`).
  8. Publishes the batch to the ingest buffer; a buffer the stream
     backend cannot accept is 503 `ingest_buffer_unavailable` with
     a `Retry-After` header.

On success the handler records the ingest-lag, byte, and record
metrics and returns 202 with an `IngestReceipt` carrying the
server acceptance timestamp and the accepted record count.

DE-PERSONALISATION: audit-ingest labels carry Domain, Project, and
Node — never an Identity subject or email. PII that a record
inadvertently carries is the customer's ingest-side responsibility.

DEFERRED-WIRING POSTURE: when no observability ingest backend is
configured (empty `PLEXSPHERE_OBS_NATS_URL`) the production
composition root leaves this surface disabled and every request
returns 501 with `code: observability_ingest_not_provisioned` so
log scrapers can alert on the deferred-wiring state.


### Example


```python
import plexsphere
from plexsphere.models.audit_event import AuditEvent
from plexsphere.models.ingest_receipt import IngestReceipt
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
    api_instance = plexsphere.MeshApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Node identifier (UUIDv7) — the ingest scope.
    authorization = 'authorization_example' # str | `Bearer <NSK plaintext>` — the per-Node Node Secret Key issued at registration time. The NSK is bound to the Node addressed by the path `id`; a credential belonging to a different Node surfaces as 403 `node_id_mismatch`. 
    audit_event = [plexsphere.AuditEvent()] # List[AuditEvent] | 
    x_plexsphere_sent_at = '2013-10-20T19:20:30+01:00' # datetime | RFC 3339 timestamp at which plexd dispatched the batch. The handler requires it and refuses a missing or unparseable value with 400 `ingest_sent_at_invalid`; it drives the ingest-lag metric the platform records on acceptance.  (optional)

    try:
        # Ingest a batch of normalised audit events for a Node.
        api_response = api_instance.post_node_audit(id, authorization, audit_event, x_plexsphere_sent_at=x_plexsphere_sent_at)
        print("The response of MeshApi->post_node_audit:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling MeshApi->post_node_audit: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Node identifier (UUIDv7) — the ingest scope. | 
 **authorization** | **str**| &#x60;Bearer &lt;NSK plaintext&gt;&#x60; — the per-Node Node Secret Key issued at registration time. The NSK is bound to the Node addressed by the path &#x60;id&#x60;; a credential belonging to a different Node surfaces as 403 &#x60;node_id_mismatch&#x60;.  | 
 **audit_event** | [**List[AuditEvent]**](AuditEvent.md)|  | 
 **x_plexsphere_sent_at** | **datetime**| RFC 3339 timestamp at which plexd dispatched the batch. The handler requires it and refuses a missing or unparseable value with 400 &#x60;ingest_sent_at_invalid&#x60;; it drives the ingest-lag metric the platform records on acceptance.  | [optional] 

### Return type

[**IngestReceipt**](IngestReceipt.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/x-ndjson
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**202** | Batch accepted and handed to the ingest buffer. The &#x60;IngestReceipt&#x60; body carries the server acceptance timestamp and the accepted record count.  |  -  |
**400** | The request failed a structural gate. The Problem body&#39;s &#x60;code&#x60; field disambiguates the cause:    - &#x60;ingest_sent_at_invalid&#x60; — the &#x60;X-Plexsphere-Sent-At&#x60;     header is missing or not an RFC 3339 timestamp.   - &#x60;ingest_encoding_invalid&#x60; — the gzip stream failed to     inflate.   - &#x60;ingest_batch_malformed&#x60; — a line is not a JSON object, the     batch is empty, or a line is missing a required field or     carries a &#x60;source&#x60; outside the closed set.  |  -  |
**401** | NSK in the &#x60;Authorization: Bearer&#x60; header is missing, malformed, or has been revoked. The Problem body&#39;s &#x60;code&#x60; field is &#x60;unauthorized&#x60; for a missing or malformed credential and &#x60;nsk_revoked&#x60; for a revoked one, so log scrapers can distinguish credential revocation from a generic auth failure.  |  -  |
**403** | The NSK authenticates successfully but belongs to a different Node than the path &#x60;id&#x60;. The PermissionDenied body&#39;s &#x60;code&#x60; field is &#x60;node_id_mismatch&#x60; so a leaked NSK cannot be replayed against a sibling Node.  |  -  |
**413** | The batch exceeded a size cap. The Problem body&#39;s &#x60;code&#x60; field is &#x60;ingest_body_too_large&#x60; when the compressed wire body or the inflated batch exceeds its byte cap, or &#x60;ingest_batch_too_many_records&#x60; when the batch carries more records than the per-batch cap.  |  -  |
**415** | The &#x60;Content-Encoding&#x60; header names an encoding other than &#x60;gzip&#x60; or &#x60;identity&#x60;. The Problem body&#39;s &#x60;code&#x60; field is &#x60;ingest_encoding_unsupported&#x60;.  |  -  |
**429** | A quota bucket is exhausted. The Problem body&#39;s &#x60;code&#x60; field is &#x60;per_node_rate_limited&#x60; when the per-Node byte budget is exhausted or &#x60;capacity_exceeded&#x60; when the per-Domain byte budget is exhausted, and the &#x60;Retry-After&#x60; response header carries the number of seconds the caller should wait before retrying.  |  * Retry-After - Seconds the caller should wait before retrying the batch.  <br>  |
**501** | The observability ingest surface is not yet provisioned in this build. The Problem body&#39;s &#x60;code&#x60; field is &#x60;observability_ingest_not_provisioned&#x60; so log scrapers can alert on the deferred-wiring state. Resolves once an observability ingest backend (&#x60;PLEXSPHERE_OBS_NATS_URL&#x60;) is configured and the ingest Service is wired into the v1 handler dependency cone.  |  -  |
**503** | The ingest buffer could not accept the batch — the stream backend is unreachable or saturated. The Problem body&#39;s &#x60;code&#x60; field is &#x60;ingest_buffer_unavailable&#x60;, and the &#x60;Retry-After&#x60; response header carries the number of seconds the caller should wait before retrying.  |  * Retry-After - Seconds the caller should wait before retrying the batch.  <br>  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **post_node_heartbeat**
> HeartbeatResponse post_node_heartbeat(id, authorization, heartbeat_request)

Record a Node liveness heartbeat and return reconcile/rotate hints.

Accepts a per-Node liveness heartbeat from plexd and updates the
reachability projection that drives the mesh-wide health view
. The handler:

  1. Authenticates the caller against the Node Secret Key (NSK)
     plaintext supplied in the `Authorization: Bearer` header,
     rejecting revoked credentials with 401.
  2. Asserts that the NSK belongs to the Node addressed by the
     path `id`, refusing cross-Node use with 403
     `node_id_mismatch` so a leaked NSK cannot be replayed
     against a sibling Node.
  3. Validates the request body — `client_now` MUST be within
     60s of server now (otherwise 400 `clock_skew`),
     `binary_checksum` MUST be present and decode to a 32-byte
     SHA-256 digest (otherwise 400 `binary_checksum_empty`)
     .
  4. Persists the heartbeat fact and updates the per-Node
     reachability state-machine (`healthy` → `stale` after 90s,
     `stale` → `unreachable` after 300s) so the projection at
     `GET /v1/nodes/{id}/reachability` reflects the new fact
     on the next read.

The 200 response carries `accepted_at` (server timestamp at
commit) and two reconciliation flags. `reconcile` defaults to
`false` and later stories flip it when the controller wants
plexd to issue a fresh reconciliation pull. `rotate_keys` is
load-bearing: it is set to `true` whenever a `peer_key_rotation`
row is pending for the heartbeating Node, telling the caller to
generate a fresh Curve25519 keypair and complete the rotation
via `POST /v1/keys/rotate`.

DEFERRED-WIRING POSTURE: until the production
composition root supplies the NSK validator, ReachabilityRepo,
and clock-skew evaluator the handler depends on, every request
to this endpoint returns 501 with `code:
heartbeat_not_provisioned` so log scrapers can alert on the
deferred-wiring state. See docs/architecture/mesh-event-bus-roadmap.md for the
deferred work tracking.


### Example


```python
import plexsphere
from plexsphere.models.heartbeat_request import HeartbeatRequest
from plexsphere.models.heartbeat_response import HeartbeatResponse
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
    api_instance = plexsphere.MeshApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Node identifier (UUIDv7) — the heartbeat scope.
    authorization = 'authorization_example' # str | `Bearer <NSK plaintext>` — the per-Node Node Secret Key issued at registration time. The NSK is bound to the Node addressed by the path `id`; a credential belonging to a different Node surfaces as 403 `node_id_mismatch` . 
    heartbeat_request = {"client_now":"2026-04-27T10:15:30Z","binary_checksum":"5e884898da28047151d0e56f8dc6292773603d0d6aabbdd62a11ef721d1542d8","binary_version":"plexd-v0.4.2-ge5f3a1c","nat_summary":{}} # HeartbeatRequest | 

    try:
        # Record a Node liveness heartbeat and return reconcile/rotate hints.
        api_response = api_instance.post_node_heartbeat(id, authorization, heartbeat_request)
        print("The response of MeshApi->post_node_heartbeat:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling MeshApi->post_node_heartbeat: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Node identifier (UUIDv7) — the heartbeat scope. | 
 **authorization** | **str**| &#x60;Bearer &lt;NSK plaintext&gt;&#x60; — the per-Node Node Secret Key issued at registration time. The NSK is bound to the Node addressed by the path &#x60;id&#x60;; a credential belonging to a different Node surfaces as 403 &#x60;node_id_mismatch&#x60; .  | 
 **heartbeat_request** | [**HeartbeatRequest**](HeartbeatRequest.md)|  | 

### Return type

[**HeartbeatResponse**](HeartbeatResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Heartbeat accepted. The reachability projection has been advanced and the response carries any reconcile / key- rotation hints the controller wants the caller to act on .  |  -  |
**400** | Heartbeat body failed structural validation. The Problem body&#39;s &#x60;code&#x60; field disambiguates the cause and is one of the following exhaustive enum values:    - &#x60;clock_skew&#x60; — &#x60;client_now&#x60; drifts more than 60 seconds     from server now.   - &#x60;binary_checksum_empty&#x60; — &#x60;binary_checksum&#x60; is missing     or does not decode to a 32-byte SHA-256 digest     .   - &#x60;binary_version_empty&#x60; — &#x60;binary_version&#x60; is missing     or empty after trimming whitespace.   - &#x60;malformed_heartbeat_request&#x60; — request body cannot be     decoded as a HeartbeatRequest envelope (e.g. invalid     JSON, unknown field).  Generated TypeScript / Go clients can therefore exhaustively switch on &#x60;code&#x60; without a fall-through arm .  |  -  |
**401** | NSK in the &#x60;Authorization: Bearer&#x60; header is missing, malformed, or has been revoked. The Problem body&#39;s &#x60;code&#x60; field is &#x60;nsk_revoked&#x60; so log scrapers can distinguish credential revocation from generic auth failures .  |  -  |
**403** | The NSK in the &#x60;Authorization: Bearer&#x60; header authenticates successfully but belongs to a different Node than the path &#x60;id&#x60;. The PermissionDenied body&#39;s &#x60;code&#x60; field is &#x60;node_id_mismatch&#x60; so a leaked NSK cannot be replayed against a sibling Node.  |  -  |
**501** | The heartbeat surface is not yet provisioned in this build. The Problem body&#39;s &#x60;code&#x60; field is &#x60;heartbeat_not_provisioned&#x60; so log scrapers can alert on the deferred-wiring state. Resolves once the NSK validator, ReachabilityRepo, and clock-skew evaluator are wired into the v1 handler dependency cone (see docs/architecture/mesh-event-bus-roadmap.md).  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **post_node_integrity_violations**
> IntegrityViolationsResponse post_node_integrity_violations(id, authorization, integrity_violations_request)

Ingest a batch of integrity-violation reports for a Node.

Accepts a batch of `IntegrityViolation` reports from plexd
describing tamper-evidence divergences the agent detected on
the local Node: a mismatched binary checksum, a tampered Lua
hook checksum, or a rotated SSH host-key fingerprint. The
handler:

  1. Authenticates the caller against the Node Secret Key (NSK)
     plaintext supplied in the `Authorization: Bearer` header,
     refusing missing or revoked credentials with 401.
  2. Asserts that the NSK belongs to the Node addressed by the
     path `id`, refusing cross-Node use with 403
     `node_id_mismatch` so a leaked NSK cannot be replayed
     against a sibling Node.
  3. Caps the request body at 32 KiB; oversize bodies receive
     413 `integrity_violations_body_too_large` without ever
     touching the JSON decoder.
  4. Decodes the body with `DisallowUnknownFields`; a malformed
     envelope or an unknown field surfaces as 400
     `malformed_integrity_violations_request`.
  5. Canonicalises every entry through the
     `tenancy.NewIntegrityViolation` value-object constructor
     which enforces every per-violation invariant: `kind` in
     the closed set `{binary_checksum, hook_checksum,
     ssh_host_key}`, `detected_by` in the closed set
     `{startup_scan, inotify, pre_dispatch}`, `artifact_id`
     non-empty after trimming, `observed_checksum`/
     `expected_checksum` exactly 32 bytes for the checksum
     kinds, `observed_fingerprint`/`expected_fingerprint`
     matching `SHA256:<base64>` for the `ssh_host_key` kind,
     and the per-kind column-mismatch guard that rejects a
     checksum kind carrying fingerprint fields and vice versa.
  6. Hands the canonical batch to the Node aggregate's
     `RecordIntegrityViolations` method which enforces the
     batch-level invariants: at least one entry
     (`integrity_violations_empty`) and at most 128 entries
     (`integrity_violations_too_many`).
  7. Persists every violation row and appends a single
     `integrity_alert` outbox event inside one transaction so
     the operator signal and the audit evidence land together
     or neither lands.

The 202 response carries `accepted_at` (server commit
timestamp) and `violation_count` (echo of the persisted batch
size) so the agent can record the receipt and reconcile with
its local replay-queue. The response carries no payload echo
and no per-row identifiers; the operator UI consumes the
persisted rows from the dedicated audit and integrity surfaces.

DEFERRED-WIRING POSTURE: until the production composition root
supplies the IntegrityViolationsRecorder, NSKResolver, and
NodeRepo ports the handler depends on, every request to this
endpoint returns 501 with `code:
integrity_violations_not_provisioned` so log scrapers can
alert on the deferred-wiring state.


### Example


```python
import plexsphere
from plexsphere.models.integrity_violations_request import IntegrityViolationsRequest
from plexsphere.models.integrity_violations_response import IntegrityViolationsResponse
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
    api_instance = plexsphere.MeshApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Node identifier (UUIDv7) — the reporting Node.
    authorization = 'authorization_example' # str | `Bearer <NSK plaintext>` — the per-Node Node Secret Key issued at registration time. The NSK is bound to the Node addressed by the path `id`; a credential belonging to a different Node surfaces as 403 `node_id_mismatch`. 
    integrity_violations_request = {"violations":[{"kind":"binary_checksum","detected_by":"startup_scan","artifact_id":"plexd","observed_checksum":"XohImNooBHFR0OVvjcYpJ3NgPQ1qq73WKhHvch0VQtg=","expected_checksum":"n4bQgYhMfWWaL+qgxVrQFaO/TxsrC4Is0V1sFbDwCgg="},{"kind":"ssh_host_key","detected_by":"inotify","artifact_id":"ssh_host_ed25519_key.pub","observed_fingerprint":"SHA256:6QGz1Q4iE2zG5p2N3oRZb8ZsT4nKqJgY3oZmP8eFvWk=","expected_fingerprint":"SHA256:7QGz1Q4iE2zG5p2N3oRZb8ZsT4nKqJgY3oZmP8eFvWk="}]} # IntegrityViolationsRequest | 

    try:
        # Ingest a batch of integrity-violation reports for a Node.
        api_response = api_instance.post_node_integrity_violations(id, authorization, integrity_violations_request)
        print("The response of MeshApi->post_node_integrity_violations:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling MeshApi->post_node_integrity_violations: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Node identifier (UUIDv7) — the reporting Node. | 
 **authorization** | **str**| &#x60;Bearer &lt;NSK plaintext&gt;&#x60; — the per-Node Node Secret Key issued at registration time. The NSK is bound to the Node addressed by the path &#x60;id&#x60;; a credential belonging to a different Node surfaces as 403 &#x60;node_id_mismatch&#x60;.  | 
 **integrity_violations_request** | [**IntegrityViolationsRequest**](IntegrityViolationsRequest.md)|  | 

### Return type

[**IntegrityViolationsResponse**](IntegrityViolationsResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**202** | Integrity-violation batch accepted. Every violation row has been persisted and the dedicated &#x60;integrity_alert&#x60; outbox event has been appended inside the same transaction.  |  -  |
**400** | Integrity-violation body failed structural validation. The Problem body&#39;s &#x60;code&#x60; field disambiguates the cause and is one of the following exhaustive enum values:    - &#x60;malformed_integrity_violations_request&#x60; — request     body cannot be decoded as an IntegrityViolationsRequest     envelope (invalid JSON, unknown field, missing     required field).   - &#x60;integrity_violations_empty&#x60; — the &#x60;violations&#x60; array     is empty after decoding.   - &#x60;integrity_violations_too_many&#x60; — the &#x60;violations&#x60;     array exceeds the per-batch cap of 128 entries.   - &#x60;integrity_violation_kind_invalid&#x60; — an entry&#39;s &#x60;kind&#x60;     is not in &#x60;{binary_checksum, hook_checksum,     ssh_host_key}&#x60;.   - &#x60;integrity_violation_kind_mismatch&#x60; — a &#x60;binary_checksum&#x60;     or &#x60;hook_checksum&#x60; entry carries a fingerprint field,     or an &#x60;ssh_host_key&#x60; entry carries a checksum field.   - &#x60;integrity_violation_detected_by_invalid&#x60; — an entry&#39;s     &#x60;detected_by&#x60; is not in &#x60;{startup_scan, inotify,     pre_dispatch}&#x60;.   - &#x60;integrity_violation_artifact_id_empty&#x60; — an entry&#39;s     &#x60;artifact_id&#x60; is empty or whitespace-only.   - &#x60;integrity_violation_checksum_invalid&#x60; — an entry&#39;s     &#x60;observed_checksum&#x60; or &#x60;expected_checksum&#x60; is missing     or does not decode to exactly 32 bytes.   - &#x60;integrity_violation_host_key_fingerprint_invalid&#x60; —     an entry&#39;s &#x60;observed_fingerprint&#x60; or     &#x60;expected_fingerprint&#x60; is non-empty and does not match     the canonical &#x60;SHA256:&lt;base64&gt;&#x60; form.  Generated TypeScript / Go clients can therefore exhaustively switch on &#x60;code&#x60; without a fall-through arm.  |  -  |
**401** | NSK in the &#x60;Authorization: Bearer&#x60; header is missing, malformed, or has been revoked. The Problem body&#39;s &#x60;code&#x60; field disambiguates the cause:    - &#x60;nsk_revoked&#x60; — the NSK has been revoked by the NSK     middleware (the normal production path; rejection     happens before the handler runs).   - &#x60;unauthorized&#x60; — the handler&#39;s defensive arm fired     because the NSK middleware did not attach a Node to     the request context (a misconfigured route, or the     middleware was bypassed).  Generated TypeScript / Go clients can exhaustively switch on &#x60;code&#x60; without a fall-through arm. The example below illustrates the canonical &#x60;nsk_revoked&#x60; path.  |  -  |
**403** | The NSK in the &#x60;Authorization: Bearer&#x60; header authenticates successfully but belongs to a different Node than the path &#x60;id&#x60;. The PermissionDenied body&#39;s &#x60;code&#x60; field is &#x60;node_id_mismatch&#x60; so a leaked NSK cannot be replayed against a sibling Node.  |  -  |
**404** | No Node row resolves for the path &#x60;id&#x60; — the row was concurrently deleted between the NSK middleware&#39;s admission and the aggregate write. The Problem body&#39;s &#x60;code&#x60; field is &#x60;integrity_violations_node_not_found&#x60;.  |  -  |
**413** | Request body exceeded the 32 KiB ceiling enforced by the handler. The Problem body&#39;s &#x60;code&#x60; field is &#x60;integrity_violations_body_too_large&#x60; so the handler can refuse a pathological payload without ever invoking the JSON decoder.  |  -  |
**500** | Internal server error. A persistence or outbox failure that does not map onto a specific 4xx surfaces here. The body is an RFC 9457 Problem without a stable &#x60;code&#x60; field so operators learn the cause from logs rather than the wire.  |  -  |
**501** | The integrity-violations ingest surface is not yet provisioned in this build. The Problem body&#39;s &#x60;code&#x60; field is &#x60;integrity_violations_not_provisioned&#x60; so log scrapers can alert on the deferred-wiring state. Resolves once the IntegrityViolationsRecorder, NSKResolver, and NodeRepo ports are wired into the v1 handler dependency cone.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **post_node_keys_rotate**
> RotationTriggerResponse post_node_keys_rotate(id)

Trigger a mesh-key rotation for a Node.

Asks the control plane to dispatch a `rotate_keys` command to
the addressed Node so it generates a fresh Curve25519 keypair
without re-enrolling from scratch. An operator calls this when
a Node's mesh key is aged or possibly compromised. The
handler:

  1. Authenticates the caller and rejects requests without a
     resolved Principal with 401.
  2. Checks the operator-action ReBAC relation `node-operate`
     on `node:<id>` BEFORE any existence check, so an
     unauthorised caller cannot probe Node existence via the
     404/403 timing differential. A caller lacking the
     relation receives 403 `insufficient_relation` and an
     audit row with outcome `insufficient_relation`.
  3. Records a pending `peer_key_rotation` row and appends
     exactly one `rotate_keys` outbox event in a single
     transaction. A second trigger against a Node that already
     has a pending rotation returns the existing rotation
     handle and appends no second event — the operation is
     idempotent.

The 200 response carries the rotation handle: the
`peer_key_rotation` row id, the live Peer id, and
`already_pending` (true when the response reflects a
pre-existing in-flight rotation rather than a fresh request).

DEFERRED-WIRING POSTURE: until the production composition root
supplies the RotationRequester and the ReBAC RelationChecker
the handler depends on, every request to this endpoint returns
501 with `code: node_keys_rotate_not_provisioned` so log
scrapers can alert on the deferred-wiring state.


### Example


```python
import plexsphere
from plexsphere.models.rotation_trigger_response import RotationTriggerResponse
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
    api_instance = plexsphere.MeshApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Node identifier (UUIDv7) — the rotation scope.

    try:
        # Trigger a mesh-key rotation for a Node.
        api_response = api_instance.post_node_keys_rotate(id)
        print("The response of MeshApi->post_node_keys_rotate:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling MeshApi->post_node_keys_rotate: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Node identifier (UUIDv7) — the rotation scope. | 

### Return type

[**RotationTriggerResponse**](RotationTriggerResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Rotation request recorded. A pending &#x60;peer_key_rotation&#x60; row exists and a &#x60;rotate_keys&#x60; command has been dispatched (or was already in flight — see &#x60;already_pending&#x60;).  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the &#x60;node-operate&#x60; ReBAC relation on the addressed Node and may not trigger a key rotation. The PermissionDenied body&#39;s &#x60;reason&#x60; field is &#x60;insufficient_relation&#x60;. Surfaced before any existence check so the endpoint cannot be used as a Node-id oracle.  |  -  |
**404** | Node id does not match any Node in this Domain. Surfaced only after the authorisation gate has passed so the endpoint cannot be used as a Node-id oracle. The Problem body&#39;s &#x60;code&#x60; field is &#x60;node_not_found&#x60;.  |  -  |
**500** | Internal server error. An authorization-check fault or a rotation-request persistence failure surfaces here.  |  -  |
**501** | The key-rotation trigger surface is not yet provisioned in this build. The Problem body&#39;s &#x60;code&#x60; field is &#x60;node_keys_rotate_not_provisioned&#x60; so log scrapers can alert on the deferred-wiring state. Resolves once the RotationRequester and the ReBAC RelationChecker are wired into the v1 handler dependency cone.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **post_node_logs**
> IngestReceipt post_node_logs(id, authorization, log_line, x_plexsphere_sent_at=x_plexsphere_sent_at)

Ingest a batch of structured log lines for a Node.

Accepts a gzip-compressed newline-delimited JSON
(`application/x-ndjson`) batch of `LogLine` records — one object
per line — pushed by plexd and hands the batch to the per-signal
ingest buffer (a JetStream subject keyed by the originating Node's
Domain), from which the platform routes to Grafana Loki plus
optional SIEM forwarding. Every record is tagged server-side with
the Domain and Project of the originating Node so dashboards and
alerts scope through the platform label model; the wire body never
carries an Identity subject or email (see the de-personalisation
note below).

The handler applies the following gates in order; each maps to a
single stable `code` on an RFC 9457 Problem body so generated
clients can switch on `code` exhaustively:

  1. Authenticates the caller against the Node Secret Key (NSK)
     plaintext in the `Authorization: Bearer` header, refusing a
     missing, malformed, or revoked credential with 401.
  2. Asserts the NSK belongs to the Node addressed by the path
     `id`, refusing cross-Node use with 403 `node_id_mismatch` so
     a leaked NSK cannot be replayed against a sibling Node.
  3. Refuses a `Content-Encoding` other than `gzip` or `identity`
     with 415 `ingest_encoding_unsupported`.
  4. Requires the `X-Plexsphere-Sent-At` header to be present and
     an RFC 3339 timestamp; a missing or unparseable value is
     refused with 400 `ingest_sent_at_invalid`. The header drives
     the ingest-lag metric.
  5. Caps the compressed wire body; a body over the cap is refused
     with 413 `ingest_body_too_large` before the gzip reader runs.
  6. Applies the per-Node then per-Domain byte-weighted quota; an
     exhausted per-Node bucket surfaces as 429
     `per_node_rate_limited` and an exhausted per-Domain bucket as
     429 `capacity_exceeded`, both carrying a `Retry-After` header.
  7. Inflates the gzip stream under a decompression cap (a stream
     that fails to inflate is 400 `ingest_encoding_invalid`; an
     inflated batch over the cap is 413 `ingest_body_too_large`)
     and structurally validates each line (a malformed line or a
     `severity` outside the closed set is 400
     `ingest_batch_malformed`; more than the per-batch record cap
     is 413 `ingest_batch_too_many_records`).
  8. Publishes the batch to the ingest buffer; a buffer the stream
     backend cannot accept is 503 `ingest_buffer_unavailable` with
     a `Retry-After` header.

On success the handler records the ingest-lag, byte, and record
metrics and returns 202 with an `IngestReceipt` carrying the
server acceptance timestamp and the accepted record count.

DE-PERSONALISATION: log labels carry Domain, Project, and Node —
never an Identity subject or email. Log lines that inadvertently
contain PII are the customer's ingest-side responsibility; an
optional per-Domain regex redaction filter, disabled by default,
can be enabled at ingest.

DEFERRED-WIRING POSTURE: when no observability ingest backend is
configured (empty `PLEXSPHERE_OBS_NATS_URL`) the production
composition root leaves this surface disabled and every request
returns 501 with `code: observability_ingest_not_provisioned` so
log scrapers can alert on the deferred-wiring state.


### Example


```python
import plexsphere
from plexsphere.models.ingest_receipt import IngestReceipt
from plexsphere.models.log_line import LogLine
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
    api_instance = plexsphere.MeshApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Node identifier (UUIDv7) — the ingest scope.
    authorization = 'authorization_example' # str | `Bearer <NSK plaintext>` — the per-Node Node Secret Key issued at registration time. The NSK is bound to the Node addressed by the path `id`; a credential belonging to a different Node surfaces as 403 `node_id_mismatch`. 
    log_line = [plexsphere.LogLine()] # List[LogLine] | 
    x_plexsphere_sent_at = '2013-10-20T19:20:30+01:00' # datetime | RFC 3339 timestamp at which plexd dispatched the batch. The handler requires it and refuses a missing or unparseable value with 400 `ingest_sent_at_invalid`; it drives the ingest-lag metric the platform records on acceptance.  (optional)

    try:
        # Ingest a batch of structured log lines for a Node.
        api_response = api_instance.post_node_logs(id, authorization, log_line, x_plexsphere_sent_at=x_plexsphere_sent_at)
        print("The response of MeshApi->post_node_logs:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling MeshApi->post_node_logs: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Node identifier (UUIDv7) — the ingest scope. | 
 **authorization** | **str**| &#x60;Bearer &lt;NSK plaintext&gt;&#x60; — the per-Node Node Secret Key issued at registration time. The NSK is bound to the Node addressed by the path &#x60;id&#x60;; a credential belonging to a different Node surfaces as 403 &#x60;node_id_mismatch&#x60;.  | 
 **log_line** | [**List[LogLine]**](LogLine.md)|  | 
 **x_plexsphere_sent_at** | **datetime**| RFC 3339 timestamp at which plexd dispatched the batch. The handler requires it and refuses a missing or unparseable value with 400 &#x60;ingest_sent_at_invalid&#x60;; it drives the ingest-lag metric the platform records on acceptance.  | [optional] 

### Return type

[**IngestReceipt**](IngestReceipt.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/x-ndjson
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**202** | Batch accepted and handed to the ingest buffer. The &#x60;IngestReceipt&#x60; body carries the server acceptance timestamp and the accepted record count.  |  -  |
**400** | The request failed a structural gate. The Problem body&#39;s &#x60;code&#x60; field disambiguates the cause:    - &#x60;ingest_sent_at_invalid&#x60; — the &#x60;X-Plexsphere-Sent-At&#x60;     header is missing or not an RFC 3339 timestamp.   - &#x60;ingest_encoding_invalid&#x60; — the gzip stream failed to     inflate.   - &#x60;ingest_batch_malformed&#x60; — a line is not a JSON object, the     batch is empty, or a line is missing a required field or     carries a &#x60;severity&#x60; outside the closed set.  |  -  |
**401** | NSK in the &#x60;Authorization: Bearer&#x60; header is missing, malformed, or has been revoked. The Problem body&#39;s &#x60;code&#x60; field is &#x60;unauthorized&#x60; for a missing or malformed credential and &#x60;nsk_revoked&#x60; for a revoked one, so log scrapers can distinguish credential revocation from a generic auth failure.  |  -  |
**403** | The NSK authenticates successfully but belongs to a different Node than the path &#x60;id&#x60;. The PermissionDenied body&#39;s &#x60;code&#x60; field is &#x60;node_id_mismatch&#x60; so a leaked NSK cannot be replayed against a sibling Node.  |  -  |
**413** | The batch exceeded a size cap. The Problem body&#39;s &#x60;code&#x60; field is &#x60;ingest_body_too_large&#x60; when the compressed wire body or the inflated batch exceeds its byte cap, or &#x60;ingest_batch_too_many_records&#x60; when the batch carries more records than the per-batch cap.  |  -  |
**415** | The &#x60;Content-Encoding&#x60; header names an encoding other than &#x60;gzip&#x60; or &#x60;identity&#x60;. The Problem body&#39;s &#x60;code&#x60; field is &#x60;ingest_encoding_unsupported&#x60;.  |  -  |
**429** | A quota bucket is exhausted. The Problem body&#39;s &#x60;code&#x60; field is &#x60;per_node_rate_limited&#x60; when the per-Node byte budget is exhausted or &#x60;capacity_exceeded&#x60; when the per-Domain byte budget is exhausted, and the &#x60;Retry-After&#x60; response header carries the number of seconds the caller should wait before retrying.  |  * Retry-After - Seconds the caller should wait before retrying the batch.  <br>  |
**501** | The observability ingest surface is not yet provisioned in this build. The Problem body&#39;s &#x60;code&#x60; field is &#x60;observability_ingest_not_provisioned&#x60; so log scrapers can alert on the deferred-wiring state. Resolves once an observability ingest backend (&#x60;PLEXSPHERE_OBS_NATS_URL&#x60;) is configured and the ingest Service is wired into the v1 handler dependency cone.  |  -  |
**503** | The ingest buffer could not accept the batch — the stream backend is unreachable or saturated. The Problem body&#39;s &#x60;code&#x60; field is &#x60;ingest_buffer_unavailable&#x60;, and the &#x60;Retry-After&#x60; response header carries the number of seconds the caller should wait before retrying.  |  * Retry-After - Seconds the caller should wait before retrying the batch.  <br>  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **post_node_metrics**
> IngestReceipt post_node_metrics(id, authorization, metric_sample, x_plexsphere_sent_at=x_plexsphere_sent_at)

Ingest a batch of metric samples for a Node.

Accepts a gzip-compressed JSON array of `MetricSample` records
pushed by plexd and hands the batch to the per-signal ingest
buffer — a JetStream subject keyed by the originating Node's
Domain — from which the platform fans out to Grafana Mimir via
Prometheus remote-write. Every record is tagged server-side with
the Domain and Project of the originating Node so dashboards and
alerts scope through the platform label model; the wire body never
carries an Identity subject or email (see the de-personalisation
note below).

The handler applies the following gates in order; each maps to a
single stable `code` on an RFC 9457 Problem body so generated
clients can switch on `code` exhaustively:

  1. Authenticates the caller against the Node Secret Key (NSK)
     plaintext in the `Authorization: Bearer` header, refusing a
     missing, malformed, or revoked credential with 401.
  2. Asserts the NSK belongs to the Node addressed by the path
     `id`, refusing cross-Node use with 403 `node_id_mismatch` so
     a leaked NSK cannot be replayed against a sibling Node.
  3. Refuses a `Content-Encoding` other than `gzip` or `identity`
     with 415 `ingest_encoding_unsupported`.
  4. Requires the `X-Plexsphere-Sent-At` header to be present and
     an RFC 3339 timestamp; a missing or unparseable value is
     refused with 400 `ingest_sent_at_invalid`. The header drives
     the ingest-lag metric.
  5. Caps the compressed wire body; a body over the cap is refused
     with 413 `ingest_body_too_large` before the gzip reader runs.
  6. Applies the per-Node then per-Domain byte-weighted quota; an
     exhausted per-Node bucket surfaces as 429
     `per_node_rate_limited` and an exhausted per-Domain bucket as
     429 `capacity_exceeded`, both carrying a `Retry-After` header.
  7. Inflates the gzip stream under a decompression cap (a stream
     that fails to inflate is 400 `ingest_encoding_invalid`; an
     inflated batch over the cap is 413 `ingest_body_too_large`)
     and structurally validates each record (a malformed record or
     a `group` outside the closed set is 400
     `ingest_batch_malformed`; more than the per-batch record cap
     is 413 `ingest_batch_too_many_records`).
  8. Publishes the batch to the ingest buffer; a buffer the stream
     backend cannot accept is 503 `ingest_buffer_unavailable` with
     a `Retry-After` header.

On success the handler records the ingest-lag, byte, and record
metrics and returns 202 with an `IngestReceipt` carrying the
server acceptance timestamp and the accepted record count.

DE-PERSONALISATION: metric labels carry Domain, Project, Node, and
(where relevant) workload identifier — never an Identity subject or
email. PII that a record inadvertently carries is the customer's
ingest-side responsibility.

DEFERRED-WIRING POSTURE: when no observability ingest backend is
configured (empty `PLEXSPHERE_OBS_NATS_URL`) the production
composition root leaves this surface disabled and every request
returns 501 with `code: observability_ingest_not_provisioned` so
log scrapers can alert on the deferred-wiring state.


### Example


```python
import plexsphere
from plexsphere.models.ingest_receipt import IngestReceipt
from plexsphere.models.metric_sample import MetricSample
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
    api_instance = plexsphere.MeshApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Node identifier (UUIDv7) — the ingest scope.
    authorization = 'authorization_example' # str | `Bearer <NSK plaintext>` — the per-Node Node Secret Key issued at registration time. The NSK is bound to the Node addressed by the path `id`; a credential belonging to a different Node surfaces as 403 `node_id_mismatch`. 
    metric_sample = [{"group":"node_resources","name":"cpu_seconds_total","value":12345.6,"labels":{"cpu":"0","mode":"user"},"timestamp":"2026-04-27T10:15:30Z"}] # List[MetricSample] | 
    x_plexsphere_sent_at = '2013-10-20T19:20:30+01:00' # datetime | RFC 3339 timestamp at which plexd dispatched the batch. The handler requires it and refuses a missing or unparseable value with 400 `ingest_sent_at_invalid`; it drives the ingest-lag metric the platform records on acceptance.  (optional)

    try:
        # Ingest a batch of metric samples for a Node.
        api_response = api_instance.post_node_metrics(id, authorization, metric_sample, x_plexsphere_sent_at=x_plexsphere_sent_at)
        print("The response of MeshApi->post_node_metrics:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling MeshApi->post_node_metrics: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Node identifier (UUIDv7) — the ingest scope. | 
 **authorization** | **str**| &#x60;Bearer &lt;NSK plaintext&gt;&#x60; — the per-Node Node Secret Key issued at registration time. The NSK is bound to the Node addressed by the path &#x60;id&#x60;; a credential belonging to a different Node surfaces as 403 &#x60;node_id_mismatch&#x60;.  | 
 **metric_sample** | [**List[MetricSample]**](MetricSample.md)|  | 
 **x_plexsphere_sent_at** | **datetime**| RFC 3339 timestamp at which plexd dispatched the batch. The handler requires it and refuses a missing or unparseable value with 400 &#x60;ingest_sent_at_invalid&#x60;; it drives the ingest-lag metric the platform records on acceptance.  | [optional] 

### Return type

[**IngestReceipt**](IngestReceipt.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**202** | Batch accepted and handed to the ingest buffer. The &#x60;IngestReceipt&#x60; body carries the server acceptance timestamp and the accepted record count.  |  -  |
**400** | The request failed a structural gate. The Problem body&#39;s &#x60;code&#x60; field disambiguates the cause:    - &#x60;ingest_sent_at_invalid&#x60; — the &#x60;X-Plexsphere-Sent-At&#x60;     header is missing or not an RFC 3339 timestamp.   - &#x60;ingest_encoding_invalid&#x60; — the gzip stream failed to     inflate.   - &#x60;ingest_batch_malformed&#x60; — the decompressed body is not a     JSON array, is empty, or a record is missing a required     field or carries a &#x60;group&#x60; outside the closed set.  |  -  |
**401** | NSK in the &#x60;Authorization: Bearer&#x60; header is missing, malformed, or has been revoked. The Problem body&#39;s &#x60;code&#x60; field is &#x60;unauthorized&#x60; for a missing or malformed credential and &#x60;nsk_revoked&#x60; for a revoked one, so log scrapers can distinguish credential revocation from a generic auth failure.  |  -  |
**403** | The NSK authenticates successfully but belongs to a different Node than the path &#x60;id&#x60;. The PermissionDenied body&#39;s &#x60;code&#x60; field is &#x60;node_id_mismatch&#x60; so a leaked NSK cannot be replayed against a sibling Node.  |  -  |
**413** | The batch exceeded a size cap. The Problem body&#39;s &#x60;code&#x60; field is &#x60;ingest_body_too_large&#x60; when the compressed wire body or the inflated batch exceeds its byte cap, or &#x60;ingest_batch_too_many_records&#x60; when the batch carries more records than the per-batch cap.  |  -  |
**415** | The &#x60;Content-Encoding&#x60; header names an encoding other than &#x60;gzip&#x60; or &#x60;identity&#x60;. The Problem body&#39;s &#x60;code&#x60; field is &#x60;ingest_encoding_unsupported&#x60;.  |  -  |
**429** | A quota bucket is exhausted. The Problem body&#39;s &#x60;code&#x60; field is &#x60;per_node_rate_limited&#x60; when the per-Node byte budget is exhausted or &#x60;capacity_exceeded&#x60; when the per-Domain byte budget is exhausted, and the &#x60;Retry-After&#x60; response header carries the number of seconds the caller should wait before retrying.  |  * Retry-After - Seconds the caller should wait before retrying the batch.  <br>  |
**501** | The observability ingest surface is not yet provisioned in this build. The Problem body&#39;s &#x60;code&#x60; field is &#x60;observability_ingest_not_provisioned&#x60; so log scrapers can alert on the deferred-wiring state. Resolves once an observability ingest backend (&#x60;PLEXSPHERE_OBS_NATS_URL&#x60;) is configured and the ingest Service is wired into the v1 handler dependency cone.  |  -  |
**503** | The ingest buffer could not accept the batch — the stream backend is unreachable or saturated. The Problem body&#39;s &#x60;code&#x60; field is &#x60;ingest_buffer_unavailable&#x60;, and the &#x60;Retry-After&#x60; response header carries the number of seconds the caller should wait before retrying.  |  * Retry-After - Seconds the caller should wait before retrying the batch.  <br>  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **put_node_capabilities**
> CapabilityManifestResponse put_node_capabilities(id, authorization, capability_manifest_request)

Record the per-Node capability manifest snapshot.

Accepts a CapabilityManifest snapshot from plexd describing the
running agent binary, its SSH host-key fingerprint, and any
hooks the agent advertises. The handler:

  1. Authenticates the caller against the Node Secret Key (NSK)
     plaintext supplied in the `Authorization: Bearer` header,
     refusing missing or revoked credentials with 401.
  2. Asserts that the NSK belongs to the Node addressed by the
     path `id`, refusing cross-Node use with 403
     `node_id_mismatch` so a leaked NSK cannot be replayed
     against a sibling Node.
  3. Caps the request body at 32 KiB; oversize bodies receive
     413 `capabilities_body_too_large` without ever touching
     the JSON decoder.
  4. Decodes the body with `DisallowUnknownFields`; a malformed
     envelope or an unknown field surfaces as 400
     `malformed_capabilities_request`.
  5. Canonicalises the envelope through the
     `tenancy.NewCapabilityManifest` value-object constructor
     which enforces every manifest invariant: `binary_version`
     non-empty, `binary_checksum` exactly 32 bytes,
     `ssh_host_key_fingerprint` empty or matching
     `SHA256:<base64>`, no duplicate hook names, hook count
     within the per-manifest cap, and every hook's `checksum`
     exactly 32 bytes.
  6. Persists the snapshot inside a single transaction. When
     the supplied manifest differs from the prior persisted
     row, the recorder appends a `NodeCapabilitiesUpdated`
     outbox event in the same transaction so downstream
     integrity correlators and dashboard projectors observe
     the change atomically.

The 200 response carries `accepted_at` (server commit
timestamp), the alphabetically-sorted `fields_changed` list of
manifest fields that transitioned versus the prior snapshot,
and the dedicated `host_key_changed` flag so downstream
security correlators can branch on SSH host-key transitions
without parsing `fields_changed`. An idempotent PUT (no diff)
emits an empty `fields_changed` array and `host_key_changed:
false`.

DEFERRED-WIRING POSTURE: until the production composition root
supplies the CapabilitiesRecorder, NSKResolver, and NodeRepo
ports the handler depends on, every request to this endpoint
returns 501 with `code: capabilities_not_provisioned` so log
scrapers can alert on the deferred-wiring state.


### Example


```python
import plexsphere
from plexsphere.models.capability_manifest_request import CapabilityManifestRequest
from plexsphere.models.capability_manifest_response import CapabilityManifestResponse
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
    api_instance = plexsphere.MeshApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Node identifier (UUIDv7) — the capability manifest scope.
    authorization = 'authorization_example' # str | `Bearer <NSK plaintext>` — the per-Node Node Secret Key issued at registration time. The NSK is bound to the Node addressed by the path `id`; a credential belonging to a different Node surfaces as 403 `node_id_mismatch`. 
    capability_manifest_request = {"binary_version":"plexd-v0.4.2-ge5f3a1c","binary_checksum":"XohImNooBHFR0OVvjcYpJ3NgPQ1qq73WKhHvch0VQtg=","ssh_host_key_fingerprint":"SHA256:6QGz1Q4iE2zG5p2N3oRZb8ZsT4nKqJgY3oZmP8eFvWk=","declared_hooks":[{"name":"post-install","checksum":"XohImNooBHFR0OVvjcYpJ3NgPQ1qq73WKhHvch0VQtg="}],"plexd_hooks":[{"name":"nightly-backup","image_digest":"sha256:e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855","parameters":{"retention":"7d"},"timeout_seconds":300,"sandbox":true}]} # CapabilityManifestRequest | 

    try:
        # Record the per-Node capability manifest snapshot.
        api_response = api_instance.put_node_capabilities(id, authorization, capability_manifest_request)
        print("The response of MeshApi->put_node_capabilities:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling MeshApi->put_node_capabilities: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Node identifier (UUIDv7) — the capability manifest scope. | 
 **authorization** | **str**| &#x60;Bearer &lt;NSK plaintext&gt;&#x60; — the per-Node Node Secret Key issued at registration time. The NSK is bound to the Node addressed by the path &#x60;id&#x60;; a credential belonging to a different Node surfaces as 403 &#x60;node_id_mismatch&#x60;.  | 
 **capability_manifest_request** | [**CapabilityManifestRequest**](CapabilityManifestRequest.md)|  | 

### Return type

[**CapabilityManifestResponse**](CapabilityManifestResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Capability manifest accepted. The persisted snapshot has been updated and the response carries the server commit timestamp and the diff against the prior snapshot.  |  -  |
**400** | Capability manifest body failed structural validation. The Problem body&#39;s &#x60;code&#x60; field disambiguates the cause and is one of the following exhaustive enum values:    - &#x60;binary_version_empty&#x60; — &#x60;binary_version&#x60; is missing     or empty after trimming whitespace.   - &#x60;binary_checksum_invalid&#x60; — &#x60;binary_checksum&#x60; is     missing or does not decode to a 32-byte SHA-256     digest.   - &#x60;ssh_host_key_fingerprint_invalid&#x60; —     &#x60;ssh_host_key_fingerprint&#x60; is non-empty and does not     match the canonical &#x60;SHA256:&lt;base64&gt;&#x60; form.   - &#x60;declared_hook_invalid&#x60; — &#x60;declared_hooks&#x60; contains an     entry whose &#x60;name&#x60; is empty or whose &#x60;checksum&#x60; is not     exactly 32 bytes.   - &#x60;declared_hook_duplicate&#x60; — &#x60;declared_hooks&#x60; contains     two entries with the same &#x60;name&#x60;.   - &#x60;declared_hooks_too_many&#x60; — &#x60;declared_hooks&#x60; exceeds     the per-manifest cap of 128 entries.   - &#x60;malformed_capabilities_request&#x60; — request body cannot     be decoded as a CapabilityManifestRequest envelope     (e.g. invalid JSON, unknown field, missing required     field).  Generated TypeScript / Go clients can therefore exhaustively switch on &#x60;code&#x60; without a fall-through arm.  |  -  |
**401** | NSK in the &#x60;Authorization: Bearer&#x60; header is missing, malformed, or has been revoked. The Problem body&#39;s &#x60;code&#x60; field disambiguates the cause:    - &#x60;nsk_revoked&#x60; — the NSK has been revoked by the NSK     middleware (the normal production path; rejection     happens before the handler runs).   - &#x60;unauthorized&#x60; — the handler&#39;s defensive arm fired     because the NSK middleware did not attach a Node to     the request context (a misconfigured route, or the     middleware was bypassed).  Generated TypeScript / Go clients can exhaustively switch on &#x60;code&#x60; without a fall-through arm. The example below illustrates the canonical &#x60;nsk_revoked&#x60; path.  |  -  |
**403** | The NSK in the &#x60;Authorization: Bearer&#x60; header authenticates successfully but belongs to a different Node than the path &#x60;id&#x60;. The PermissionDenied body&#39;s &#x60;code&#x60; field is &#x60;node_id_mismatch&#x60; so a leaked NSK cannot be replayed against a sibling Node.  |  -  |
**404** | No Node row resolves for the path &#x60;id&#x60; — the row was concurrently deleted between the NSK middleware&#39;s admission and the aggregate write. The Problem body&#39;s &#x60;code&#x60; field is &#x60;capabilities_node_not_found&#x60;.  |  -  |
**413** | Request body exceeded the 32 KiB ceiling enforced by the handler. The Problem body&#39;s &#x60;code&#x60; field is &#x60;capabilities_body_too_large&#x60; so the handler can refuse a pathological payload without ever invoking the JSON decoder.  |  -  |
**422** | A &#x60;plexd_hooks&#x60; entry — the discovered Kubernetes PlexdHook list — failed a value-object invariant. The structurally well-formed envelope decoded, but a discovered-hook value is semantically invalid, so the surface returns 422 rather than the 400 reserved for structural/declared-hook failures. The Problem body&#39;s &#x60;code&#x60; field disambiguates the cause and is one of the following exhaustive enum values:    - &#x60;plexd_hook_invalid&#x60; — a &#x60;plexd_hooks&#x60; entry has an     empty &#x60;name&#x60;, an &#x60;image_digest&#x60; that is not canonical     &#x60;sha256:&lt;64 lowercase hex&gt;&#x60;, or a negative     &#x60;timeout_seconds&#x60;.   - &#x60;plexd_hook_duplicate&#x60; — &#x60;plexd_hooks&#x60; contains two     entries with the same &#x60;name&#x60;.   - &#x60;plexd_hooks_too_many&#x60; — &#x60;plexd_hooks&#x60; exceeds the     per-manifest cap of 128 entries.  Generated TypeScript / Go clients can therefore exhaustively switch on &#x60;code&#x60; without a fall-through arm.  |  -  |
**501** | The capability ingest surface is not yet provisioned in this build. The Problem body&#39;s &#x60;code&#x60; field is &#x60;capabilities_not_provisioned&#x60; so log scrapers can alert on the deferred-wiring state. Resolves once the CapabilitiesRecorder, NSKResolver, and NodeRepo ports are wired into the v1 handler dependency cone.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **put_node_endpoint**
> EndpointResponse put_node_endpoint(id, authorization, endpoint_request)

Record a NAT-observed Peer endpoint observation.

Accepts a per-Node NAT endpoint observation from plexd and
stamps it onto the Peer aggregate that backs the addressed
Node. The handler:

  1. Authenticates the caller against the Node Secret Key (NSK)
     plaintext supplied in the `Authorization: Bearer` header,
     refusing missing or revoked credentials with 401.
  2. Asserts that the NSK belongs to the Node addressed by the
     path `id`, refusing cross-Node use with 403
     `node_id_mismatch` so a leaked NSK cannot be replayed
     against a sibling Node.
  3. Caps the request body at 4 KiB; oversize bodies receive
     413 `endpoint_body_too_large` without ever touching the
     JSON decoder.
  4. Validates the body — `reported_at` MUST be within 60s of
     server now (otherwise 400 `endpoint_clock_skew`),
     `endpoint` MUST parse as a `host:port` tuple in the
     RFC 6056 1..65535 port range and a non-loopback IPv4/IPv6
     address (otherwise 400 `endpoint_unparseable`), and the
     envelope MUST decode without unknown fields (otherwise
     400 `malformed_endpoint_request`).
  5. Persists the observation through the per-Domain Peer
     aggregate writer, which emits a `peer_endpoint_changed`
     outbox event when the (endpoint, port) tuple differs from
     the prior observation or transitions out of the stale
     window; identical refreshes simply refresh the freshness
     timestamp without emitting a wake-up.

The 200 response carries `accepted_at` (server commit
timestamp) and `stale_after` (`accepted_at` plus the per-Domain
endpoint TTL); plexd uses `stale_after` to schedule the next
observation before the sweeper would tombstone the endpoint.

DEFERRED-WIRING POSTURE: until the production composition root
supplies the EndpointRecorder, NSKResolver, NodeRepo, and
PeerLookup ports the handler depends on, every request to this
endpoint returns 501 with `code: endpoint_not_provisioned` so
log scrapers can alert on the deferred-wiring state.


### Example


```python
import plexsphere
from plexsphere.models.endpoint_request import EndpointRequest
from plexsphere.models.endpoint_response import EndpointResponse
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
    api_instance = plexsphere.MeshApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Node identifier (UUIDv7) — the endpoint scope.
    authorization = 'authorization_example' # str | `Bearer <NSK plaintext>` — the per-Node Node Secret Key issued at registration time. The NSK is bound to the Node addressed by the path `id`; a credential belonging to a different Node surfaces as 403 `node_id_mismatch`. 
    endpoint_request = {"endpoint":"203.0.113.7:51820","nat_type":"port_restricted","reported_at":"2026-04-27T10:15:30Z"} # EndpointRequest | 

    try:
        # Record a NAT-observed Peer endpoint observation.
        api_response = api_instance.put_node_endpoint(id, authorization, endpoint_request)
        print("The response of MeshApi->put_node_endpoint:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling MeshApi->put_node_endpoint: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Node identifier (UUIDv7) — the endpoint scope. | 
 **authorization** | **str**| &#x60;Bearer &lt;NSK plaintext&gt;&#x60; — the per-Node Node Secret Key issued at registration time. The NSK is bound to the Node addressed by the path &#x60;id&#x60;; a credential belonging to a different Node surfaces as 403 &#x60;node_id_mismatch&#x60;.  | 
 **endpoint_request** | [**EndpointRequest**](EndpointRequest.md)|  | 

### Return type

[**EndpointResponse**](EndpointResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Endpoint observation accepted. The Peer aggregate has been stamped and the response carries the server commit timestamp and the freshness deadline the agent must beat before the sweeper tombstones the endpoint.  |  -  |
**400** | Endpoint body failed structural validation. The Problem body&#39;s &#x60;code&#x60; field disambiguates the cause and is one of the following exhaustive enum values:    - &#x60;endpoint_clock_skew&#x60; — covers two semantically     distinct refusals that share one wire code:     (a) &#x60;reported_at&#x60; drifts more than 60 seconds from     server now (the handler&#39;s MaxEndpointSkew gate);     (b) &#x60;reported_at&#x60; is older than the per-Domain     endpoint TTL window so the agent&#39;s keep-alive     cadence has lapsed (the recorder&#39;s     ErrEndpointObservationStale gate). Operators can     disambiguate the two flavours from the audit-row     &#x60;reason&#x60; field — the recorder-side stale arm     stamps &#x60;reported_at older than per-Domain endpoint     TTL&#x60; while the handler-side drift arm stamps     &#x60;reported_at outside MaxEndpointSkew window&#x60;. The     agent remediation is the same in both flavours     (refresh &#x60;reported_at&#x60; from a fresh &#x60;time.Now()&#x60;     and retry), so the two share one wire code.   - &#x60;endpoint_unparseable&#x60; — &#x60;endpoint&#x60; is not a valid     &#x60;host:port&#x60; tuple, the port is outside 1..65535, or     the host is not a routable IPv4/IPv6 address.   - &#x60;malformed_endpoint_request&#x60; — request body cannot be     decoded as an EndpointRequest envelope (e.g. invalid     JSON, unknown field, missing required field).  |  -  |
**401** | NSK in the &#x60;Authorization: Bearer&#x60; header is missing, malformed, or has been revoked. The Problem body&#39;s &#x60;code&#x60; field is &#x60;nsk_revoked&#x60; so log scrapers can distinguish credential revocation from generic auth failures.  |  -  |
**403** | The NSK in the &#x60;Authorization: Bearer&#x60; header authenticates successfully but belongs to a different Node than the path &#x60;id&#x60;. The PermissionDenied body&#39;s &#x60;code&#x60; field is &#x60;node_id_mismatch&#x60; so a leaked NSK cannot be replayed against a sibling Node.  |  -  |
**404** | No live Peer row resolves for the authenticated Node — the Node was drained or has never been bound to a Peer. The Problem body&#39;s &#x60;code&#x60; field is &#x60;endpoint_peer_not_found&#x60;.  |  -  |
**410** | The target Peer was deregistered between the handler&#39;s admission gates and the recorder&#39;s UPDATE — a narrow race that surfaces as 410 Gone so the agent learns to stop reporting against a row that no longer accepts observations. The Problem body&#39;s &#x60;code&#x60; field is &#x60;endpoint_peer_gone&#x60;.  |  -  |
**413** | Request body exceeded the 4 KiB ceiling enforced by the handler. The Problem body&#39;s &#x60;code&#x60; field is &#x60;endpoint_body_too_large&#x60; so the handler can refuse a pathological payload without ever invoking the JSON decoder.  |  -  |
**501** | The endpoint intake surface is not yet provisioned in this build. The Problem body&#39;s &#x60;code&#x60; field is &#x60;endpoint_not_provisioned&#x60; so log scrapers can alert on the deferred-wiring state. Resolves once the EndpointRecorder, NSKResolver, NodeRepo, and PeerLookup ports are wired into the v1 handler dependency cone.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **put_node_state_report**
> NodeStateReportResponse put_node_state_report(id, key, authorization, node_state_report_request)

Record an upstream node-state report entry for a Node.

Records one upstream, workload-authored node-state report entry
for the addressed Node under the addressed key. The entry is
stored with `kind = report`; report entries are Node-authored
telemetry and — unlike the platform-owned metadata/data entries —
do NOT fan out a `node_state_updated` event.

The caller authenticates with its Node Secret Key (NSK) in the
`Authorization: Bearer` header; the handler asserts the resolved
NSK Node matches the path `id` so a Node may only write its own
reports. The stored entry surfaces in the `reports` bucket of the
`state`/`reports` blocks on the next
`GET /v1/nodes/{id}/state` reconciliation pull.

DEFERRED-WIRING POSTURE: when the node-state service is not wired
into this build every request returns 501 with
`code: reports_not_provisioned`.


### Example


```python
import plexsphere
from plexsphere.models.node_state_report_request import NodeStateReportRequest
from plexsphere.models.node_state_report_response import NodeStateReportResponse
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
    api_instance = plexsphere.MeshApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Node identifier (UUIDv7) — the report-write scope.
    key = 'key_example' # str | Report entry key. Lower-case, starts with a letter, and is limited to letters, digits, dot, hyphen, and underscore (1 to 128 characters). 
    authorization = 'authorization_example' # str | `Bearer <NSK plaintext>` — the per-Node Node Secret Key issued at registration time. Only a Node proving possession of its NSK may write a report; a missing, malformed, or revoked credential surfaces as 401. 
    node_state_report_request = plexsphere.NodeStateReportRequest() # NodeStateReportRequest | 

    try:
        # Record an upstream node-state report entry for a Node.
        api_response = api_instance.put_node_state_report(id, key, authorization, node_state_report_request)
        print("The response of MeshApi->put_node_state_report:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling MeshApi->put_node_state_report: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Node identifier (UUIDv7) — the report-write scope. | 
 **key** | **str**| Report entry key. Lower-case, starts with a letter, and is limited to letters, digits, dot, hyphen, and underscore (1 to 128 characters).  | 
 **authorization** | **str**| &#x60;Bearer &lt;NSK plaintext&gt;&#x60; — the per-Node Node Secret Key issued at registration time. Only a Node proving possession of its NSK may write a report; a missing, malformed, or revoked credential surfaces as 401.  | 
 **node_state_report_request** | [**NodeStateReportRequest**](NodeStateReportRequest.md)|  | 

### Return type

[**NodeStateReportResponse**](NodeStateReportResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | The report entry was accepted and durably recorded. The body echoes the stored key and the server accept timestamp.  |  -  |
**400** | The request body is malformed, the key violates the entry grammar, the value exceeds the 4096-byte ceiling, or the per-Node report quota is exhausted. The Problem body&#39;s &#x60;code&#x60; field disambiguates the cause (&#x60;invalid_report&#x60; or &#x60;report_quota_exceeded&#x60;).  |  -  |
**401** | NSK in the &#x60;Authorization: Bearer&#x60; header is missing, malformed, or has been revoked. The Problem body&#39;s &#x60;code&#x60; field is &#x60;unauthorized&#x60;.  |  -  |
**403** | The NSK authenticates but resolves to a different Node than the path &#x60;id&#x60; — a Node may only write its own reports. The PermissionDenied body&#39;s &#x60;code&#x60; field is &#x60;node_id_mismatch&#x60; so a leaked NSK cannot be replayed against a sibling Node.  |  -  |
**404** | No Node resolves for the addressed &#x60;id&#x60;. The Problem body&#39;s &#x60;code&#x60; field is &#x60;node_not_found&#x60;.  |  -  |
**413** | The request body exceeds the handler&#39;s defensive size cap. The Problem body&#39;s &#x60;code&#x60; field is &#x60;payload_too_large&#x60;.  |  -  |
**501** | The node-state service is not yet provisioned in this build. The Problem body&#39;s &#x60;code&#x60; field is &#x60;reports_not_provisioned&#x60; so log scrapers can alert on the deferred-wiring state.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

