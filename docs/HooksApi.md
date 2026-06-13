# plexsphere.HooksApi

All URIs are relative to *http://localhost*

Method | HTTP request | Description
------------- | ------------- | -------------
[**delete_managed_push**](HooksApi.md#delete_managed_push) | **DELETE** /v1/domains/{domainId}/managed-push | Detach a Domain&#39;s managed-push target.
[**get_managed_hook_push**](HooksApi.md#get_managed_hook_push) | **GET** /v1/domains/{domainId}/managed-push/hooks/{pushId} | Read a single recorded managed-hook push.
[**get_managed_push**](HooksApi.md#get_managed_push) | **GET** /v1/domains/{domainId}/managed-push | Read the managed-push target configured for a Domain.
[**list_hooks**](HooksApi.md#list_hooks) | **GET** /v1/hooks | List discovered hooks across Domains.
[**push_managed_hook**](HooksApi.md#push_managed_hook) | **POST** /v1/domains/{domainId}/managed-push/hooks | Apply a PlexdHook to a Domain&#39;s managed-push cluster.
[**put_managed_push**](HooksApi.md#put_managed_push) | **PUT** /v1/domains/{domainId}/managed-push | Attach or replace a Domain&#39;s managed-push target.
[**rollback_managed_hook_push**](HooksApi.md#rollback_managed_hook_push) | **POST** /v1/domains/{domainId}/managed-push/hooks/{pushId}:rollback | Roll back a recorded managed-hook push.


# **delete_managed_push**
> delete_managed_push(domain_id)

Detach a Domain's managed-push target.

Detaches the managed-push target for a Domain, removing the sealed
kubeconfig and disabling further managed pushes. The handler runs
the managed-push write ReBAC check on the addressed Domain before
the delete, then removes the target and returns 204. A Domain with
no managed-push target receives 404 `managed_push_not_configured`.

DEFERRED-WIRING POSTURE: until the production composition root
supplies the managed-push service bundle, every request returns 501.


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
    api_instance = plexsphere.HooksApi(api_client)
    domain_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Domain. The Platform Audit Log is per-Domain by residency contract — cross-Domain decisions land in EACH affected Domain's chain, never on a shared system chain . Every audit endpoint requires the Domain identifier in the path. 

    try:
        # Detach a Domain's managed-push target.
        api_instance.delete_managed_push(domain_id)
    except Exception as e:
        print("Exception when calling HooksApi->delete_managed_push: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **domain_id** | **UUID**| Owning Domain. The Platform Audit Log is per-Domain by residency contract — cross-Domain decisions land in EACH affected Domain&#39;s chain, never on a shared system chain . Every audit endpoint requires the Domain identifier in the path.  | 

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
**204** | Managed-push target detached. No response body.  |  -  |
**400** | The path &#x60;{domainId}&#x60; was not a non-zero UUID. The Problem body&#39;s &#x60;code&#x60; field is &#x60;invalid_domain_id&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the managed-push write ReBAC relation on the addressed Domain.  |  -  |
**404** | The Domain has no managed-push target configured. The Problem body&#39;s &#x60;code&#x60; field is &#x60;managed_push_not_configured&#x60;.  |  -  |
**500** | Internal server error. |  -  |
**501** | The managed-push surface is not yet provisioned in this build, so log scrapers can alert on the deferred-wiring state.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **get_managed_hook_push**
> ManagedHookPush get_managed_hook_push(domain_id, push_id)

Read a single recorded managed-hook push.

Returns the read projection of a single recorded managed-hook push
by its identifier. The handler runs the managed-push read ReBAC
check on the addressed Domain before the read; a push that does not
exist on the Domain receives 404 `push_not_found`.

DEFERRED-WIRING POSTURE: until the production composition root
supplies the managed-push service bundle, every request returns 501.


### Example


```python
import plexsphere
from plexsphere.models.managed_hook_push import ManagedHookPush
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
    domain_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Domain. The Platform Audit Log is per-Domain by residency contract — cross-Domain decisions land in EACH affected Domain's chain, never on a shared system chain . Every audit endpoint requires the Domain identifier in the path. 
    push_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Recorded managed-hook push identifier (UUIDv7). Bound on `/v1/domains/{domainId}/managed-push/hooks/{pushId}` and its `:rollback` sub-resource for the single-push read and rollback. 

    try:
        # Read a single recorded managed-hook push.
        api_response = api_instance.get_managed_hook_push(domain_id, push_id)
        print("The response of HooksApi->get_managed_hook_push:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling HooksApi->get_managed_hook_push: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **domain_id** | **UUID**| Owning Domain. The Platform Audit Log is per-Domain by residency contract — cross-Domain decisions land in EACH affected Domain&#39;s chain, never on a shared system chain . Every audit endpoint requires the Domain identifier in the path.  | 
 **push_id** | **UUID**| Recorded managed-hook push identifier (UUIDv7). Bound on &#x60;/v1/domains/{domainId}/managed-push/hooks/{pushId}&#x60; and its &#x60;:rollback&#x60; sub-resource for the single-push read and rollback.  | 

### Return type

[**ManagedHookPush**](ManagedHookPush.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Read projection of the recorded managed-hook push.  |  -  |
**400** | The path &#x60;{domainId}&#x60; or &#x60;{pushId}&#x60; was not a non-zero UUID. The Problem body&#39;s &#x60;code&#x60; field is &#x60;invalid_domain_id&#x60; for a malformed Domain identifier.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the managed-push read ReBAC relation on the addressed Domain.  |  -  |
**404** | No recorded push with that identifier exists on the Domain. The Problem body&#39;s &#x60;code&#x60; field is &#x60;push_not_found&#x60;.  |  -  |
**500** | Internal server error. |  -  |
**501** | The managed-push surface is not yet provisioned in this build, so log scrapers can alert on the deferred-wiring state.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **get_managed_push**
> ManagedPushTarget get_managed_push(domain_id)

Read the managed-push target configured for a Domain.

Returns the read projection of the opt-in managed-push target a
Domain operator attached for the Domain — the cluster the platform
applies PlexdHook objects to on the operator's behalf. The
projection is display-only: it carries the target's enabled flag,
the API-server URL, a hex SHA-256 fingerprint of the kubeconfig
plaintext, and the create/update timestamps. It NEVER carries the
kubeconfig ciphertext or plaintext — the plaintext is sealed at
rest and never echoed back over the API.

The handler:

  1. Authenticates the caller and rejects requests without a
     resolved Principal with 401.
  2. Checks the managed-push read ReBAC relation on the addressed
     Domain BEFORE reading the target so the endpoint cannot be
     used as a Domain-id oracle; an unauthorised caller receives
     403.
  3. Returns the target projection with 200 when a target is
     configured, or 404 `managed_push_not_configured` when the
     Domain has no managed-push target.

DEFERRED-WIRING POSTURE: until the production composition root
supplies the managed-push service bundle, every request returns 501
so log scrapers can alert on the deferred-wiring state.


### Example


```python
import plexsphere
from plexsphere.models.managed_push_target import ManagedPushTarget
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
    domain_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Domain. The Platform Audit Log is per-Domain by residency contract — cross-Domain decisions land in EACH affected Domain's chain, never on a shared system chain . Every audit endpoint requires the Domain identifier in the path. 

    try:
        # Read the managed-push target configured for a Domain.
        api_response = api_instance.get_managed_push(domain_id)
        print("The response of HooksApi->get_managed_push:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling HooksApi->get_managed_push: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **domain_id** | **UUID**| Owning Domain. The Platform Audit Log is per-Domain by residency contract — cross-Domain decisions land in EACH affected Domain&#39;s chain, never on a shared system chain . Every audit endpoint requires the Domain identifier in the path.  | 

### Return type

[**ManagedPushTarget**](ManagedPushTarget.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Managed-push target projection for the addressed Domain.  |  -  |
**400** | The path &#x60;{domainId}&#x60; was not a non-zero UUID. The Problem body&#39;s &#x60;code&#x60; field is &#x60;invalid_domain_id&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the managed-push read ReBAC relation on the addressed Domain. Surfaced before the target read so the endpoint cannot be used as a Domain-id oracle.  |  -  |
**404** | The Domain has no managed-push target configured. The Problem body&#39;s &#x60;code&#x60; field is &#x60;managed_push_not_configured&#x60;.  |  -  |
**500** | Internal server error. |  -  |
**501** | The managed-push surface is not yet provisioned in this build, so log scrapers can alert on the deferred-wiring state. Resolves once the managed-push service bundle is wired into the v1 handler dependency cone.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

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

# **push_managed_hook**
> ManagedHookPush push_managed_hook(domain_id, managed_hook_push_request)

Apply a PlexdHook to a Domain's managed-push cluster.

Applies one PlexdHook object to the Domain's managed-push cluster
on the operator's behalf, recording the push so it can be read back
or rolled back. The handler runs the managed-push write ReBAC check
on the addressed Domain before any cluster write, validates the
PlexdHook spec, then applies it to the target cluster.

The response is the read projection of the recorded push; it NEVER
echoes prior object content — only a `had_prior_state` boolean
recording whether the apply replaced an existing object, so a
subsequent rollback knows whether to restore or delete.

DEFERRED-WIRING POSTURE: until the production composition root
supplies the managed-push service bundle, every request returns 501.


### Example


```python
import plexsphere
from plexsphere.models.managed_hook_push import ManagedHookPush
from plexsphere.models.managed_hook_push_request import ManagedHookPushRequest
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
    domain_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Domain. The Platform Audit Log is per-Domain by residency contract — cross-Domain decisions land in EACH affected Domain's chain, never on a shared system chain . Every audit endpoint requires the Domain identifier in the path. 
    managed_hook_push_request = {"namespace":"platform-hooks","name":"nightly-backup","image_digest":"sha256:e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855","parameters":{"schedule":"0 2 * * *"},"timeout_seconds":300,"sandbox":true} # ManagedHookPushRequest | 

    try:
        # Apply a PlexdHook to a Domain's managed-push cluster.
        api_response = api_instance.push_managed_hook(domain_id, managed_hook_push_request)
        print("The response of HooksApi->push_managed_hook:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling HooksApi->push_managed_hook: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **domain_id** | **UUID**| Owning Domain. The Platform Audit Log is per-Domain by residency contract — cross-Domain decisions land in EACH affected Domain&#39;s chain, never on a shared system chain . Every audit endpoint requires the Domain identifier in the path.  | 
 **managed_hook_push_request** | [**ManagedHookPushRequest**](ManagedHookPushRequest.md)|  | 

### Return type

[**ManagedHookPush**](ManagedHookPush.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**201** | PlexdHook applied. Body is the read projection of the recorded push.  |  -  |
**400** | The path &#x60;{domainId}&#x60; was not a non-zero UUID, or the request body was malformed. The Problem body&#39;s &#x60;code&#x60; field is &#x60;invalid_domain_id&#x60; for a malformed path identifier.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the managed-push write ReBAC relation on the addressed Domain.  |  -  |
**404** | The Domain has no managed-push target configured. The Problem body&#39;s &#x60;code&#x60; field is &#x60;managed_push_not_configured&#x60;.  |  -  |
**409** | The push could not be admitted. The Problem body&#39;s &#x60;code&#x60; field is &#x60;managed_push_disabled&#x60; when the Domain&#39;s target exists but is disabled, or &#x60;managed_push_conflict&#x60; when a concurrent push against the same hook is in flight.  |  -  |
**422** | The PlexdHook spec failed validation. The Problem body&#39;s &#x60;code&#x60; field is &#x60;plexd_hook_invalid&#x60;.  |  -  |
**502** | The managed-push cluster could not be reached to apply the object. The Problem body&#39;s &#x60;code&#x60; field is &#x60;cluster_unreachable&#x60;.  |  -  |
**500** | Internal server error. |  -  |
**501** | The managed-push surface is not yet provisioned in this build, so log scrapers can alert on the deferred-wiring state.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **put_managed_push**
> ManagedPushTarget put_managed_push(domain_id, managed_push_attach_request)

Attach or replace a Domain's managed-push target.

Attaches the opt-in managed-push target for a Domain, or replaces
the existing target's kubeconfig and enabled flag. The request
carries a base64-encoded kubeconfig and the enabled flag; the
plaintext is validated, sealed at rest, and NEVER echoed back —
the 200 response is the display-only target projection.

The handler:

  1. Authenticates the caller and rejects requests without a
     resolved Principal with 401.
  2. Checks the managed-push write ReBAC relation on the addressed
     Domain BEFORE any persistence write; an unauthorised caller
     receives 403.
  3. Decodes and validates the kubeconfig. Only embedded, in-band
     credentials are accepted: a token, or `client-certificate-data`
     / `client-key-data`, with the CA supplied as
     `certificate-authority-data`. An oversized payload is rejected
     with 413 `kubeconfig_too_large`; a kubeconfig that fails
     validation is rejected with 422 `kubeconfig_invalid`. Validation
     rejects an `exec` or `auth-provider` credential plugin, any
     file-path credential (`tokenFile`, `client-certificate`,
     `client-key`), a `certificate-authority` file path, a
     `proxy-url`, a non-https server, and `insecure-skip-tls-verify`
     — the control plane never reads operator-named local files nor
     dials an operator-supplied proxy.
  4. Seals the plaintext and persists the target, returning the
     display-only projection with 200.

DEFERRED-WIRING POSTURE: until the production composition root
supplies the managed-push service bundle, every request returns 501.


### Example


```python
import plexsphere
from plexsphere.models.managed_push_attach_request import ManagedPushAttachRequest
from plexsphere.models.managed_push_target import ManagedPushTarget
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
    domain_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Domain. The Platform Audit Log is per-Domain by residency contract — cross-Domain decisions land in EACH affected Domain's chain, never on a shared system chain . Every audit endpoint requires the Domain identifier in the path. 
    managed_push_attach_request = {"kubeconfig_b64":"YXBpVmVyc2lvbjogdjEKa2luZDogQ29uZmlnCg==","enabled":true} # ManagedPushAttachRequest | 

    try:
        # Attach or replace a Domain's managed-push target.
        api_response = api_instance.put_managed_push(domain_id, managed_push_attach_request)
        print("The response of HooksApi->put_managed_push:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling HooksApi->put_managed_push: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **domain_id** | **UUID**| Owning Domain. The Platform Audit Log is per-Domain by residency contract — cross-Domain decisions land in EACH affected Domain&#39;s chain, never on a shared system chain . Every audit endpoint requires the Domain identifier in the path.  | 
 **managed_push_attach_request** | [**ManagedPushAttachRequest**](ManagedPushAttachRequest.md)|  | 

### Return type

[**ManagedPushTarget**](ManagedPushTarget.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Managed-push target attached or replaced. Body is the display-only target projection; the kubeconfig plaintext is never echoed back.  |  -  |
**400** | The path &#x60;{domainId}&#x60; was not a non-zero UUID, or the request body was malformed. The Problem body&#39;s &#x60;code&#x60; field is &#x60;invalid_domain_id&#x60; for a malformed path identifier.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the managed-push write ReBAC relation on the addressed Domain.  |  -  |
**404** | The addressed Domain does not exist or is not visible. The Problem body&#39;s &#x60;code&#x60; field is &#x60;domain_not_found&#x60;.  |  -  |
**413** | The decoded kubeconfig exceeds the per-target size ceiling. The Problem body&#39;s &#x60;code&#x60; field is &#x60;kubeconfig_too_large&#x60;.  |  -  |
**422** | The kubeconfig failed validation — for example it carries an &#x60;exec&#x60; or &#x60;auth-provider&#x60; credential plugin, or is otherwise malformed. The Problem body&#39;s &#x60;code&#x60; field is &#x60;kubeconfig_invalid&#x60;.  |  -  |
**500** | Internal server error. |  -  |
**501** | The managed-push surface is not yet provisioned in this build, so log scrapers can alert on the deferred-wiring state.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **rollback_managed_hook_push**
> ManagedHookPush rollback_managed_hook_push(domain_id, push_id)

Roll back a recorded managed-hook push.

Rolls back a previously recorded managed-hook push: restores the
prior object state when the apply replaced an existing object, or
deletes the applied object when it had no prior state. The handler
runs the managed-push write ReBAC check on the addressed Domain
before the cluster write.

The `:rollback` colon-verb mirrors the
`/v1/projects/{project_id}/executions:dispatch` idiom — rollback is
a side-effecting cluster operation, not an idempotent field write.
A push that has already been rolled back receives 409
`push_already_rolled_back`.

DEFERRED-WIRING POSTURE: until the production composition root
supplies the managed-push service bundle, every request returns 501.


### Example


```python
import plexsphere
from plexsphere.models.managed_hook_push import ManagedHookPush
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
    domain_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Domain. The Platform Audit Log is per-Domain by residency contract — cross-Domain decisions land in EACH affected Domain's chain, never on a shared system chain . Every audit endpoint requires the Domain identifier in the path. 
    push_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Recorded managed-hook push identifier (UUIDv7). Bound on `/v1/domains/{domainId}/managed-push/hooks/{pushId}` and its `:rollback` sub-resource for the single-push read and rollback. 

    try:
        # Roll back a recorded managed-hook push.
        api_response = api_instance.rollback_managed_hook_push(domain_id, push_id)
        print("The response of HooksApi->rollback_managed_hook_push:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling HooksApi->rollback_managed_hook_push: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **domain_id** | **UUID**| Owning Domain. The Platform Audit Log is per-Domain by residency contract — cross-Domain decisions land in EACH affected Domain&#39;s chain, never on a shared system chain . Every audit endpoint requires the Domain identifier in the path.  | 
 **push_id** | **UUID**| Recorded managed-hook push identifier (UUIDv7). Bound on &#x60;/v1/domains/{domainId}/managed-push/hooks/{pushId}&#x60; and its &#x60;:rollback&#x60; sub-resource for the single-push read and rollback.  | 

### Return type

[**ManagedHookPush**](ManagedHookPush.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Push rolled back. Body is the read projection of the push with &#x60;status: rolled_back&#x60;.  |  -  |
**400** | The path &#x60;{domainId}&#x60; or &#x60;{pushId}&#x60; was not a non-zero UUID. The Problem body&#39;s &#x60;code&#x60; field is &#x60;invalid_domain_id&#x60; for a malformed Domain identifier.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the managed-push write ReBAC relation on the addressed Domain.  |  -  |
**404** | No recorded push with that identifier exists on the Domain. The Problem body&#39;s &#x60;code&#x60; field is &#x60;push_not_found&#x60;.  |  -  |
**409** | The push has already been rolled back. The Problem body&#39;s &#x60;code&#x60; field is &#x60;push_already_rolled_back&#x60;.  |  -  |
**502** | The managed-push cluster could not be reached to roll back the object. The Problem body&#39;s &#x60;code&#x60; field is &#x60;cluster_unreachable&#x60;.  |  -  |
**500** | Internal server error. |  -  |
**501** | The managed-push surface is not yet provisioned in this build, so log scrapers can alert on the deferred-wiring state.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

