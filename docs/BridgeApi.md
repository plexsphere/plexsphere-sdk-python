# plexsphere.BridgeApi

All URIs are relative to *http://localhost*

Method | HTTP request | Description
------------- | ------------- | -------------
[**configure_bridge_relay**](BridgeApi.md#configure_bridge_relay) | **PUT** /v1/projects/{project_id}/resources/{resource_id}/bridge/relay | Configure the bridge relay (last-writer-wins replace).
[**create_bridge_ingress_rule**](BridgeApi.md#create_bridge_ingress_rule) | **POST** /v1/projects/{project_id}/resources/{resource_id}/bridge/ingress | Create a bridge public-ingress rule.
[**create_bridge_site_to_site_tunnel**](BridgeApi.md#create_bridge_site_to_site_tunnel) | **POST** /v1/projects/{project_id}/resources/{resource_id}/bridge/site-to-site | Create a bridge site-to-site tunnel.
[**create_bridge_user_access_provider**](BridgeApi.md#create_bridge_user_access_provider) | **POST** /v1/projects/{project_id}/resources/{resource_id}/bridge/user-access | Create a bridge user-access provider.
[**delete_bridge_ingress_rule**](BridgeApi.md#delete_bridge_ingress_rule) | **DELETE** /v1/projects/{project_id}/resources/{resource_id}/bridge/ingress/{slug} | Delete a bridge public-ingress rule.
[**delete_bridge_site_to_site_tunnel**](BridgeApi.md#delete_bridge_site_to_site_tunnel) | **DELETE** /v1/projects/{project_id}/resources/{resource_id}/bridge/site-to-site/{slug} | Delete a bridge site-to-site tunnel.
[**delete_bridge_user_access_provider**](BridgeApi.md#delete_bridge_user_access_provider) | **DELETE** /v1/projects/{project_id}/resources/{resource_id}/bridge/user-access/{slug} | Delete a bridge user-access provider.
[**get_bridge_ingress_rule**](BridgeApi.md#get_bridge_ingress_rule) | **GET** /v1/projects/{project_id}/resources/{resource_id}/bridge/ingress/{slug} | Fetch a bridge public-ingress rule by slug.
[**get_bridge_relay**](BridgeApi.md#get_bridge_relay) | **GET** /v1/projects/{project_id}/resources/{resource_id}/bridge/relay | Fetch the bridge relay configuration.
[**get_bridge_site_to_site_tunnel**](BridgeApi.md#get_bridge_site_to_site_tunnel) | **GET** /v1/projects/{project_id}/resources/{resource_id}/bridge/site-to-site/{slug} | Fetch a bridge site-to-site tunnel by slug.
[**get_bridge_user_access_provider**](BridgeApi.md#get_bridge_user_access_provider) | **GET** /v1/projects/{project_id}/resources/{resource_id}/bridge/user-access/{slug} | Fetch a bridge user-access provider by slug.
[**list_bridge_ingress_rules**](BridgeApi.md#list_bridge_ingress_rules) | **GET** /v1/projects/{project_id}/resources/{resource_id}/bridge/ingress | List the bridge public-ingress rules.
[**list_bridge_site_to_site_tunnels**](BridgeApi.md#list_bridge_site_to_site_tunnels) | **GET** /v1/projects/{project_id}/resources/{resource_id}/bridge/site-to-site | List the bridge site-to-site tunnels.
[**list_bridge_user_access_providers**](BridgeApi.md#list_bridge_user_access_providers) | **GET** /v1/projects/{project_id}/resources/{resource_id}/bridge/user-access | List the bridge user-access providers.
[**update_bridge_ingress_rule**](BridgeApi.md#update_bridge_ingress_rule) | **PATCH** /v1/projects/{project_id}/resources/{resource_id}/bridge/ingress/{slug} | Replace a bridge public-ingress rule&#39;s configuration.
[**update_bridge_site_to_site_tunnel**](BridgeApi.md#update_bridge_site_to_site_tunnel) | **PATCH** /v1/projects/{project_id}/resources/{resource_id}/bridge/site-to-site/{slug} | Replace a bridge site-to-site tunnel&#39;s configuration.
[**update_bridge_user_access_provider**](BridgeApi.md#update_bridge_user_access_provider) | **PATCH** /v1/projects/{project_id}/resources/{resource_id}/bridge/user-access/{slug} | Replace a bridge user-access provider&#39;s configuration.


# **configure_bridge_relay**
> BridgeRelayResponse configure_bridge_relay(project_id, resource_id, bridge_relay_configure_request)

Configure the bridge relay (last-writer-wins replace).

Replaces the relay configuration for the addressed bridge
Resource. The relay is a per-Resource singleton; this PUT is a
full-replace last-writer-wins write with no optimistic-concurrency
precondition. An identical-config PUT short-circuits as an
idempotent no-op. The handler runs the `manage` ReBAC check on the
addressed Resource BEFORE any persistence write and refuses a
non-bridge Resource with `409 resource_not_bridge`.


### Example


```python
import plexsphere
from plexsphere.models.bridge_relay_configure_request import BridgeRelayConfigureRequest
from plexsphere.models.bridge_relay_response import BridgeRelayResponse
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
    api_instance = plexsphere.BridgeApi(api_client)
    project_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Project (UUIDv7). Bound on `/v1/projects/{project_id}/resources` for the Resource collection — create and list. Reused by the Bridge Orchestrator surface under `/v1/projects/{project_id}/resources/{resource_id}/bridge/...`. 
    resource_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Addressed bridge Resource (UUIDv7). Bound on every `/v1/projects/{project_id}/resources/{resource_id}/bridge/...` operation. A Resource that exists but is not kind `bridge` surfaces as `409 resource_not_bridge` on every mutating call. 
    bridge_relay_configure_request = {"enabled":true,"listen_port":51820} # BridgeRelayConfigureRequest | 

    try:
        # Configure the bridge relay (last-writer-wins replace).
        api_response = api_instance.configure_bridge_relay(project_id, resource_id, bridge_relay_configure_request)
        print("The response of BridgeApi->configure_bridge_relay:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling BridgeApi->configure_bridge_relay: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **project_id** | **UUID**| Owning Project (UUIDv7). Bound on &#x60;/v1/projects/{project_id}/resources&#x60; for the Resource collection — create and list. Reused by the Bridge Orchestrator surface under &#x60;/v1/projects/{project_id}/resources/{resource_id}/bridge/...&#x60;.  | 
 **resource_id** | **UUID**| Addressed bridge Resource (UUIDv7). Bound on every &#x60;/v1/projects/{project_id}/resources/{resource_id}/bridge/...&#x60; operation. A Resource that exists but is not kind &#x60;bridge&#x60; surfaces as &#x60;409 resource_not_bridge&#x60; on every mutating call.  | 
 **bridge_relay_configure_request** | [**BridgeRelayConfigureRequest**](BridgeRelayConfigureRequest.md)|  | 

### Return type

[**BridgeRelayResponse**](BridgeRelayResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Relay configured. |  -  |
**400** | Body rejected. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;relay_port_out_of_range&#x60;, &#x60;invalid_body&#x60; }.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to manage the addressed Resource. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | The addressed Resource was not found. Body is a &#x60;Problem&#x60; with &#x60;code: resource_not_found&#x60;. The bridge handler never resolves a parent Project, so &#x60;project_not_found&#x60; is not emitted on this surface.  |  -  |
**409** | Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;resource_not_bridge&#x60; (the addressed Resource is not kind &#x60;bridge&#x60;), &#x60;bridge_port_conflict&#x60; (the relay &#x60;listen_port&#x60; is already bound by a sibling user-access provider or ingress rule within the bridge — refused by the cross-aggregate validator before persistence) }.  |  -  |
**413** | Request body exceeded the bridge body ceiling.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **create_bridge_ingress_rule**
> BridgeIngressRuleResponse create_bridge_ingress_rule(project_id, resource_id, bridge_ingress_rule_create_request)

Create a bridge public-ingress rule.

Creates a public-ingress rule on the addressed bridge Resource.
Rules are a many-aggregate keyed by `(resource_id, slug)` with an
additional `(resource_id, sni_host)` uniqueness constraint. The
handler runs the `manage` ReBAC check on the addressed Resource
BEFORE any persistence write and refuses a non-bridge Resource
with `409 resource_not_bridge`. A duplicate slug or `sni_host`
surfaces as `409 slug_conflict`.


### Example


```python
import plexsphere
from plexsphere.models.bridge_ingress_rule_create_request import BridgeIngressRuleCreateRequest
from plexsphere.models.bridge_ingress_rule_response import BridgeIngressRuleResponse
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
    api_instance = plexsphere.BridgeApi(api_client)
    project_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Project (UUIDv7). Bound on `/v1/projects/{project_id}/resources` for the Resource collection — create and list. Reused by the Bridge Orchestrator surface under `/v1/projects/{project_id}/resources/{resource_id}/bridge/...`. 
    resource_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Addressed bridge Resource (UUIDv7). Bound on every `/v1/projects/{project_id}/resources/{resource_id}/bridge/...` operation. A Resource that exists but is not kind `bridge` surfaces as `409 resource_not_bridge` on every mutating call. 
    bridge_ingress_rule_create_request = {"slug":"public-api","sni_host":"api.acme.example","target_node_id":"0190a8b8-a0c0-7a0a-8a0a-a0a0a0a0a0e3","target_port":8443,"acme_account_ref":"secret:acme/field/acme-account"} # BridgeIngressRuleCreateRequest | 

    try:
        # Create a bridge public-ingress rule.
        api_response = api_instance.create_bridge_ingress_rule(project_id, resource_id, bridge_ingress_rule_create_request)
        print("The response of BridgeApi->create_bridge_ingress_rule:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling BridgeApi->create_bridge_ingress_rule: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **project_id** | **UUID**| Owning Project (UUIDv7). Bound on &#x60;/v1/projects/{project_id}/resources&#x60; for the Resource collection — create and list. Reused by the Bridge Orchestrator surface under &#x60;/v1/projects/{project_id}/resources/{resource_id}/bridge/...&#x60;.  | 
 **resource_id** | **UUID**| Addressed bridge Resource (UUIDv7). Bound on every &#x60;/v1/projects/{project_id}/resources/{resource_id}/bridge/...&#x60; operation. A Resource that exists but is not kind &#x60;bridge&#x60; surfaces as &#x60;409 resource_not_bridge&#x60; on every mutating call.  | 
 **bridge_ingress_rule_create_request** | [**BridgeIngressRuleCreateRequest**](BridgeIngressRuleCreateRequest.md)|  | 

### Return type

[**BridgeIngressRuleResponse**](BridgeIngressRuleResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**201** | Public-ingress rule created. |  -  |
**400** | Body rejected. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;relay_port_out_of_range&#x60;, &#x60;target_node_not_in_domain&#x60;, &#x60;invalid_body&#x60; }.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to manage the addressed Resource. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | The addressed Resource was not found. Body is a &#x60;Problem&#x60; with &#x60;code: resource_not_found&#x60;. The bridge handler never resolves a parent Project, so &#x60;project_not_found&#x60; is not emitted on this surface.  |  -  |
**409** | Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;resource_not_bridge&#x60; (the addressed Resource is not kind &#x60;bridge&#x60;), &#x60;slug_conflict&#x60; (a rule with the same slug or &#x60;sni_host&#x60; already exists on this Resource), &#x60;bridge_port_conflict&#x60; (the rule&#39;s &#x60;(target_node_id, target_port)&#x60; pair is already claimed by a sibling ingress rule within the bridge — refused by the cross-aggregate validator before persistence) }.  |  -  |
**413** | Request body exceeded the bridge body ceiling.  |  -  |
**422** | The &#x60;acme_account_ref&#x60; names an ACME directory that is unreachable, returned a server error, timed out, or advertised a malformed directory document. Body is a &#x60;Problem&#x60; with &#x60;code: acme_directory_unreachable&#x60;. The request is well-formed but the certificate issuance the rule requires cannot be provisioned, so the cross-aggregate validator refuses it before persistence.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **create_bridge_site_to_site_tunnel**
> BridgeSiteToSiteTunnelResponse create_bridge_site_to_site_tunnel(project_id, resource_id, bridge_site_to_site_tunnel_create_request)

Create a bridge site-to-site tunnel.

Creates a site-to-site tunnel on the addressed bridge Resource.
Tunnels are a many-aggregate keyed by `(resource_id, slug)`. The
handler runs the `manage` ReBAC check on the addressed Resource
BEFORE any persistence write and refuses a non-bridge Resource
with `409 resource_not_bridge`. A duplicate slug surfaces as
`409 slug_conflict`.


### Example


```python
import plexsphere
from plexsphere.models.bridge_site_to_site_tunnel_create_request import BridgeSiteToSiteTunnelCreateRequest
from plexsphere.models.bridge_site_to_site_tunnel_response import BridgeSiteToSiteTunnelResponse
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
    api_instance = plexsphere.BridgeApi(api_client)
    project_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Project (UUIDv7). Bound on `/v1/projects/{project_id}/resources` for the Resource collection — create and list. Reused by the Bridge Orchestrator surface under `/v1/projects/{project_id}/resources/{resource_id}/bridge/...`. 
    resource_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Addressed bridge Resource (UUIDv7). Bound on every `/v1/projects/{project_id}/resources/{resource_id}/bridge/...` operation. A Resource that exists but is not kind `bridge` surfaces as `409 resource_not_bridge` on every mutating call. 
    bridge_site_to_site_tunnel_create_request = {"slug":"dc-east","kind":"wireguard","remote_host":"vpn.dc-east.acme.example","remote_port":51820,"auth_secret_ref":"secret:acme/field/dc-east-psk","allowed_subnets":["10.20.0.0/16"],"routing_policy":"bidirectional"} # BridgeSiteToSiteTunnelCreateRequest | 

    try:
        # Create a bridge site-to-site tunnel.
        api_response = api_instance.create_bridge_site_to_site_tunnel(project_id, resource_id, bridge_site_to_site_tunnel_create_request)
        print("The response of BridgeApi->create_bridge_site_to_site_tunnel:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling BridgeApi->create_bridge_site_to_site_tunnel: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **project_id** | **UUID**| Owning Project (UUIDv7). Bound on &#x60;/v1/projects/{project_id}/resources&#x60; for the Resource collection — create and list. Reused by the Bridge Orchestrator surface under &#x60;/v1/projects/{project_id}/resources/{resource_id}/bridge/...&#x60;.  | 
 **resource_id** | **UUID**| Addressed bridge Resource (UUIDv7). Bound on every &#x60;/v1/projects/{project_id}/resources/{resource_id}/bridge/...&#x60; operation. A Resource that exists but is not kind &#x60;bridge&#x60; surfaces as &#x60;409 resource_not_bridge&#x60; on every mutating call.  | 
 **bridge_site_to_site_tunnel_create_request** | [**BridgeSiteToSiteTunnelCreateRequest**](BridgeSiteToSiteTunnelCreateRequest.md)|  | 

### Return type

[**BridgeSiteToSiteTunnelResponse**](BridgeSiteToSiteTunnelResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**201** | Site-to-site tunnel created. |  -  |
**400** | Body rejected. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;relay_port_out_of_range&#x60;, &#x60;allowed_subnet_empty&#x60;, &#x60;secret_ref_malformed&#x60;, &#x60;invalid_body&#x60; }.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to manage the addressed Resource. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | The addressed Resource was not found. Body is a &#x60;Problem&#x60; with &#x60;code: resource_not_found&#x60;. The bridge handler never resolves a parent Project, so &#x60;project_not_found&#x60; is not emitted on this surface.  |  -  |
**409** | Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;resource_not_bridge&#x60; (the addressed Resource is not kind &#x60;bridge&#x60;), &#x60;slug_conflict&#x60; (a tunnel with the same slug already exists on this Resource), &#x60;subnet_overlap_with_mesh&#x60; (an &#x60;allowed_subnets&#x60; entry overlaps the Domain mesh CIDR), &#x60;subnet_overlap_with_tunnel&#x60; (an &#x60;allowed_subnets&#x60; entry overlaps a sibling tunnel within the bridge) — the two overlap codes are raised by the cross-aggregate validator before persistence, with the mesh check taking precedence over the sibling-tunnel check.  |  -  |
**413** | Request body exceeded the bridge body ceiling.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **create_bridge_user_access_provider**
> BridgeUserAccessProviderResponse create_bridge_user_access_provider(project_id, resource_id, bridge_user_access_provider_create_request)

Create a bridge user-access provider.

Creates a user-access provider on the addressed bridge Resource.
Providers are a many-aggregate keyed by `(resource_id, slug)`. The
handler runs the `manage` ReBAC check on the addressed Resource
BEFORE any persistence write and refuses a non-bridge Resource
with `409 resource_not_bridge`. A duplicate slug surfaces as
`409 slug_conflict`.


### Example


```python
import plexsphere
from plexsphere.models.bridge_user_access_provider_create_request import BridgeUserAccessProviderCreateRequest
from plexsphere.models.bridge_user_access_provider_response import BridgeUserAccessProviderResponse
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
    api_instance = plexsphere.BridgeApi(api_client)
    project_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Project (UUIDv7). Bound on `/v1/projects/{project_id}/resources` for the Resource collection — create and list. Reused by the Bridge Orchestrator surface under `/v1/projects/{project_id}/resources/{resource_id}/bridge/...`. 
    resource_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Addressed bridge Resource (UUIDv7). Bound on every `/v1/projects/{project_id}/resources/{resource_id}/bridge/...` operation. A Resource that exists but is not kind `bridge` surfaces as `409 resource_not_bridge` on every mutating call. 
    bridge_user_access_provider_create_request = {"slug":"field-team","kind":"wireguard","interface_name":"wg-bridge0","listen_port":51821,"max_peers":64,"auth_secret_ref":"secret:acme/field/wg-bridge0","routing_policy":{"allowed_groups":["ops"]}} # BridgeUserAccessProviderCreateRequest | 

    try:
        # Create a bridge user-access provider.
        api_response = api_instance.create_bridge_user_access_provider(project_id, resource_id, bridge_user_access_provider_create_request)
        print("The response of BridgeApi->create_bridge_user_access_provider:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling BridgeApi->create_bridge_user_access_provider: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **project_id** | **UUID**| Owning Project (UUIDv7). Bound on &#x60;/v1/projects/{project_id}/resources&#x60; for the Resource collection — create and list. Reused by the Bridge Orchestrator surface under &#x60;/v1/projects/{project_id}/resources/{resource_id}/bridge/...&#x60;.  | 
 **resource_id** | **UUID**| Addressed bridge Resource (UUIDv7). Bound on every &#x60;/v1/projects/{project_id}/resources/{resource_id}/bridge/...&#x60; operation. A Resource that exists but is not kind &#x60;bridge&#x60; surfaces as &#x60;409 resource_not_bridge&#x60; on every mutating call.  | 
 **bridge_user_access_provider_create_request** | [**BridgeUserAccessProviderCreateRequest**](BridgeUserAccessProviderCreateRequest.md)|  | 

### Return type

[**BridgeUserAccessProviderResponse**](BridgeUserAccessProviderResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**201** | User-access provider created. |  -  |
**400** | Body rejected. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;relay_port_out_of_range&#x60;, &#x60;secret_ref_malformed&#x60;, &#x60;invalid_body&#x60; }.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to manage the addressed Resource. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | The addressed Resource was not found. Body is a &#x60;Problem&#x60; with &#x60;code: resource_not_found&#x60;. The bridge handler never resolves a parent Project, so &#x60;project_not_found&#x60; is not emitted on this surface.  |  -  |
**409** | Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;resource_not_bridge&#x60; (the addressed Resource is not kind &#x60;bridge&#x60;), &#x60;slug_conflict&#x60; (a provider with the same slug already exists on this Resource), &#x60;bridge_port_conflict&#x60; (the provider &#x60;listen_port&#x60; is already bound by the relay or another provider or ingress rule within the bridge — refused by the cross-aggregate validator before persistence) }.  |  -  |
**413** | Request body exceeded the bridge body ceiling.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **delete_bridge_ingress_rule**
> delete_bridge_ingress_rule(project_id, resource_id, slug)

Delete a bridge public-ingress rule.

Deletes the public-ingress rule addressed by `{slug}`. The
handler runs the `manage` ReBAC check on the addressed Resource
BEFORE the persistence write and refuses a non-bridge Resource
with `409 resource_not_bridge`.


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
    api_instance = plexsphere.BridgeApi(api_client)
    project_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Project (UUIDv7). Bound on `/v1/projects/{project_id}/resources` for the Resource collection — create and list. Reused by the Bridge Orchestrator surface under `/v1/projects/{project_id}/resources/{resource_id}/bridge/...`. 
    resource_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Addressed bridge Resource (UUIDv7). Bound on every `/v1/projects/{project_id}/resources/{resource_id}/bridge/...` operation. A Resource that exists but is not kind `bridge` surfaces as `409 resource_not_bridge` on every mutating call. 
    slug = 'slug_example' # str | Stable slug of a bridge user-access provider, public-ingress rule, or site-to-site tunnel. Bound on the `/{slug}` sub-resource operations. Unique per `(resource_id, slug)` within its aggregate kind. 

    try:
        # Delete a bridge public-ingress rule.
        api_instance.delete_bridge_ingress_rule(project_id, resource_id, slug)
    except Exception as e:
        print("Exception when calling BridgeApi->delete_bridge_ingress_rule: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **project_id** | **UUID**| Owning Project (UUIDv7). Bound on &#x60;/v1/projects/{project_id}/resources&#x60; for the Resource collection — create and list. Reused by the Bridge Orchestrator surface under &#x60;/v1/projects/{project_id}/resources/{resource_id}/bridge/...&#x60;.  | 
 **resource_id** | **UUID**| Addressed bridge Resource (UUIDv7). Bound on every &#x60;/v1/projects/{project_id}/resources/{resource_id}/bridge/...&#x60; operation. A Resource that exists but is not kind &#x60;bridge&#x60; surfaces as &#x60;409 resource_not_bridge&#x60; on every mutating call.  | 
 **slug** | **str**| Stable slug of a bridge user-access provider, public-ingress rule, or site-to-site tunnel. Bound on the &#x60;/{slug}&#x60; sub-resource operations. Unique per &#x60;(resource_id, slug)&#x60; within its aggregate kind.  | 

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
**204** | Public-ingress rule deleted. |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to manage the addressed Resource. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | The addressed Resource or the named rule was not found. Body is a &#x60;Problem&#x60; with &#x60;code: resource_not_found&#x60;.  |  -  |
**409** | The addressed Resource exists but is not kind &#x60;bridge&#x60;. Body is a &#x60;Problem&#x60; with &#x60;code: resource_not_bridge&#x60;.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **delete_bridge_site_to_site_tunnel**
> delete_bridge_site_to_site_tunnel(project_id, resource_id, slug)

Delete a bridge site-to-site tunnel.

Deletes the site-to-site tunnel addressed by `{slug}`. The
handler runs the `manage` ReBAC check on the addressed Resource
BEFORE the persistence write and refuses a non-bridge Resource
with `409 resource_not_bridge`.


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
    api_instance = plexsphere.BridgeApi(api_client)
    project_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Project (UUIDv7). Bound on `/v1/projects/{project_id}/resources` for the Resource collection — create and list. Reused by the Bridge Orchestrator surface under `/v1/projects/{project_id}/resources/{resource_id}/bridge/...`. 
    resource_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Addressed bridge Resource (UUIDv7). Bound on every `/v1/projects/{project_id}/resources/{resource_id}/bridge/...` operation. A Resource that exists but is not kind `bridge` surfaces as `409 resource_not_bridge` on every mutating call. 
    slug = 'slug_example' # str | Stable slug of a bridge user-access provider, public-ingress rule, or site-to-site tunnel. Bound on the `/{slug}` sub-resource operations. Unique per `(resource_id, slug)` within its aggregate kind. 

    try:
        # Delete a bridge site-to-site tunnel.
        api_instance.delete_bridge_site_to_site_tunnel(project_id, resource_id, slug)
    except Exception as e:
        print("Exception when calling BridgeApi->delete_bridge_site_to_site_tunnel: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **project_id** | **UUID**| Owning Project (UUIDv7). Bound on &#x60;/v1/projects/{project_id}/resources&#x60; for the Resource collection — create and list. Reused by the Bridge Orchestrator surface under &#x60;/v1/projects/{project_id}/resources/{resource_id}/bridge/...&#x60;.  | 
 **resource_id** | **UUID**| Addressed bridge Resource (UUIDv7). Bound on every &#x60;/v1/projects/{project_id}/resources/{resource_id}/bridge/...&#x60; operation. A Resource that exists but is not kind &#x60;bridge&#x60; surfaces as &#x60;409 resource_not_bridge&#x60; on every mutating call.  | 
 **slug** | **str**| Stable slug of a bridge user-access provider, public-ingress rule, or site-to-site tunnel. Bound on the &#x60;/{slug}&#x60; sub-resource operations. Unique per &#x60;(resource_id, slug)&#x60; within its aggregate kind.  | 

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
**204** | Site-to-site tunnel deleted. |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to manage the addressed Resource. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | The addressed Resource or the named tunnel was not found. Body is a &#x60;Problem&#x60; with &#x60;code: resource_not_found&#x60;.  |  -  |
**409** | The addressed Resource exists but is not kind &#x60;bridge&#x60;. Body is a &#x60;Problem&#x60; with &#x60;code: resource_not_bridge&#x60;.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **delete_bridge_user_access_provider**
> delete_bridge_user_access_provider(project_id, resource_id, slug)

Delete a bridge user-access provider.

Deletes the user-access provider addressed by `{slug}`. The
handler runs the `manage` ReBAC check on the addressed Resource
BEFORE the persistence write and refuses a non-bridge Resource
with `409 resource_not_bridge`.


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
    api_instance = plexsphere.BridgeApi(api_client)
    project_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Project (UUIDv7). Bound on `/v1/projects/{project_id}/resources` for the Resource collection — create and list. Reused by the Bridge Orchestrator surface under `/v1/projects/{project_id}/resources/{resource_id}/bridge/...`. 
    resource_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Addressed bridge Resource (UUIDv7). Bound on every `/v1/projects/{project_id}/resources/{resource_id}/bridge/...` operation. A Resource that exists but is not kind `bridge` surfaces as `409 resource_not_bridge` on every mutating call. 
    slug = 'slug_example' # str | Stable slug of a bridge user-access provider, public-ingress rule, or site-to-site tunnel. Bound on the `/{slug}` sub-resource operations. Unique per `(resource_id, slug)` within its aggregate kind. 

    try:
        # Delete a bridge user-access provider.
        api_instance.delete_bridge_user_access_provider(project_id, resource_id, slug)
    except Exception as e:
        print("Exception when calling BridgeApi->delete_bridge_user_access_provider: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **project_id** | **UUID**| Owning Project (UUIDv7). Bound on &#x60;/v1/projects/{project_id}/resources&#x60; for the Resource collection — create and list. Reused by the Bridge Orchestrator surface under &#x60;/v1/projects/{project_id}/resources/{resource_id}/bridge/...&#x60;.  | 
 **resource_id** | **UUID**| Addressed bridge Resource (UUIDv7). Bound on every &#x60;/v1/projects/{project_id}/resources/{resource_id}/bridge/...&#x60; operation. A Resource that exists but is not kind &#x60;bridge&#x60; surfaces as &#x60;409 resource_not_bridge&#x60; on every mutating call.  | 
 **slug** | **str**| Stable slug of a bridge user-access provider, public-ingress rule, or site-to-site tunnel. Bound on the &#x60;/{slug}&#x60; sub-resource operations. Unique per &#x60;(resource_id, slug)&#x60; within its aggregate kind.  | 

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
**204** | User-access provider deleted. |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to manage the addressed Resource. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | The addressed Resource or the named provider was not found. Body is a &#x60;Problem&#x60; with &#x60;code: resource_not_found&#x60;.  |  -  |
**409** | The addressed Resource exists but is not kind &#x60;bridge&#x60;. Body is a &#x60;Problem&#x60; with &#x60;code: resource_not_bridge&#x60;.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **get_bridge_ingress_rule**
> BridgeIngressRuleResponse get_bridge_ingress_rule(project_id, resource_id, slug)

Fetch a bridge public-ingress rule by slug.

Returns the public-ingress rule addressed by `{slug}` on the
bridge Resource. The handler runs the `observe` ReBAC check on
the addressed Resource (`resource:<resource_id>`) BEFORE the
persistence read.


### Example


```python
import plexsphere
from plexsphere.models.bridge_ingress_rule_response import BridgeIngressRuleResponse
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
    api_instance = plexsphere.BridgeApi(api_client)
    project_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Project (UUIDv7). Bound on `/v1/projects/{project_id}/resources` for the Resource collection — create and list. Reused by the Bridge Orchestrator surface under `/v1/projects/{project_id}/resources/{resource_id}/bridge/...`. 
    resource_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Addressed bridge Resource (UUIDv7). Bound on every `/v1/projects/{project_id}/resources/{resource_id}/bridge/...` operation. A Resource that exists but is not kind `bridge` surfaces as `409 resource_not_bridge` on every mutating call. 
    slug = 'slug_example' # str | Stable slug of a bridge user-access provider, public-ingress rule, or site-to-site tunnel. Bound on the `/{slug}` sub-resource operations. Unique per `(resource_id, slug)` within its aggregate kind. 

    try:
        # Fetch a bridge public-ingress rule by slug.
        api_response = api_instance.get_bridge_ingress_rule(project_id, resource_id, slug)
        print("The response of BridgeApi->get_bridge_ingress_rule:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling BridgeApi->get_bridge_ingress_rule: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **project_id** | **UUID**| Owning Project (UUIDv7). Bound on &#x60;/v1/projects/{project_id}/resources&#x60; for the Resource collection — create and list. Reused by the Bridge Orchestrator surface under &#x60;/v1/projects/{project_id}/resources/{resource_id}/bridge/...&#x60;.  | 
 **resource_id** | **UUID**| Addressed bridge Resource (UUIDv7). Bound on every &#x60;/v1/projects/{project_id}/resources/{resource_id}/bridge/...&#x60; operation. A Resource that exists but is not kind &#x60;bridge&#x60; surfaces as &#x60;409 resource_not_bridge&#x60; on every mutating call.  | 
 **slug** | **str**| Stable slug of a bridge user-access provider, public-ingress rule, or site-to-site tunnel. Bound on the &#x60;/{slug}&#x60; sub-resource operations. Unique per &#x60;(resource_id, slug)&#x60; within its aggregate kind.  | 

### Return type

[**BridgeIngressRuleResponse**](BridgeIngressRuleResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Public-ingress rule found. |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to observe the addressed Resource. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | The addressed Resource or the named rule was not found. Body is a &#x60;Problem&#x60; with &#x60;code: resource_not_found&#x60;.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **get_bridge_relay**
> BridgeRelayResponse get_bridge_relay(project_id, resource_id)

Fetch the bridge relay configuration.

Returns the relay configuration for the addressed bridge Resource.
The relay is a per-Resource singleton keyed on `resource_id`. The
handler runs the `observe` ReBAC check on the addressed Resource
(`resource:<resource_id>`) BEFORE the persistence read.


### Example


```python
import plexsphere
from plexsphere.models.bridge_relay_response import BridgeRelayResponse
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
    api_instance = plexsphere.BridgeApi(api_client)
    project_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Project (UUIDv7). Bound on `/v1/projects/{project_id}/resources` for the Resource collection — create and list. Reused by the Bridge Orchestrator surface under `/v1/projects/{project_id}/resources/{resource_id}/bridge/...`. 
    resource_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Addressed bridge Resource (UUIDv7). Bound on every `/v1/projects/{project_id}/resources/{resource_id}/bridge/...` operation. A Resource that exists but is not kind `bridge` surfaces as `409 resource_not_bridge` on every mutating call. 

    try:
        # Fetch the bridge relay configuration.
        api_response = api_instance.get_bridge_relay(project_id, resource_id)
        print("The response of BridgeApi->get_bridge_relay:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling BridgeApi->get_bridge_relay: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **project_id** | **UUID**| Owning Project (UUIDv7). Bound on &#x60;/v1/projects/{project_id}/resources&#x60; for the Resource collection — create and list. Reused by the Bridge Orchestrator surface under &#x60;/v1/projects/{project_id}/resources/{resource_id}/bridge/...&#x60;.  | 
 **resource_id** | **UUID**| Addressed bridge Resource (UUIDv7). Bound on every &#x60;/v1/projects/{project_id}/resources/{resource_id}/bridge/...&#x60; operation. A Resource that exists but is not kind &#x60;bridge&#x60; surfaces as &#x60;409 resource_not_bridge&#x60; on every mutating call.  | 

### Return type

[**BridgeRelayResponse**](BridgeRelayResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Relay configuration found. |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to observe the addressed Resource. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | The addressed Resource was not found. Body is a &#x60;Problem&#x60; with &#x60;code: resource_not_found&#x60;. The bridge handler never resolves a parent Project, so &#x60;project_not_found&#x60; is not emitted on this surface.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **get_bridge_site_to_site_tunnel**
> BridgeSiteToSiteTunnelResponse get_bridge_site_to_site_tunnel(project_id, resource_id, slug)

Fetch a bridge site-to-site tunnel by slug.

Returns the site-to-site tunnel addressed by `{slug}` on the
bridge Resource. The handler runs the `observe` ReBAC check on
the addressed Resource (`resource:<resource_id>`) BEFORE the
persistence read.


### Example


```python
import plexsphere
from plexsphere.models.bridge_site_to_site_tunnel_response import BridgeSiteToSiteTunnelResponse
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
    api_instance = plexsphere.BridgeApi(api_client)
    project_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Project (UUIDv7). Bound on `/v1/projects/{project_id}/resources` for the Resource collection — create and list. Reused by the Bridge Orchestrator surface under `/v1/projects/{project_id}/resources/{resource_id}/bridge/...`. 
    resource_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Addressed bridge Resource (UUIDv7). Bound on every `/v1/projects/{project_id}/resources/{resource_id}/bridge/...` operation. A Resource that exists but is not kind `bridge` surfaces as `409 resource_not_bridge` on every mutating call. 
    slug = 'slug_example' # str | Stable slug of a bridge user-access provider, public-ingress rule, or site-to-site tunnel. Bound on the `/{slug}` sub-resource operations. Unique per `(resource_id, slug)` within its aggregate kind. 

    try:
        # Fetch a bridge site-to-site tunnel by slug.
        api_response = api_instance.get_bridge_site_to_site_tunnel(project_id, resource_id, slug)
        print("The response of BridgeApi->get_bridge_site_to_site_tunnel:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling BridgeApi->get_bridge_site_to_site_tunnel: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **project_id** | **UUID**| Owning Project (UUIDv7). Bound on &#x60;/v1/projects/{project_id}/resources&#x60; for the Resource collection — create and list. Reused by the Bridge Orchestrator surface under &#x60;/v1/projects/{project_id}/resources/{resource_id}/bridge/...&#x60;.  | 
 **resource_id** | **UUID**| Addressed bridge Resource (UUIDv7). Bound on every &#x60;/v1/projects/{project_id}/resources/{resource_id}/bridge/...&#x60; operation. A Resource that exists but is not kind &#x60;bridge&#x60; surfaces as &#x60;409 resource_not_bridge&#x60; on every mutating call.  | 
 **slug** | **str**| Stable slug of a bridge user-access provider, public-ingress rule, or site-to-site tunnel. Bound on the &#x60;/{slug}&#x60; sub-resource operations. Unique per &#x60;(resource_id, slug)&#x60; within its aggregate kind.  | 

### Return type

[**BridgeSiteToSiteTunnelResponse**](BridgeSiteToSiteTunnelResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Site-to-site tunnel found. |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to observe the addressed Resource. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | The addressed Resource or the named tunnel was not found. Body is a &#x60;Problem&#x60; with &#x60;code: resource_not_found&#x60;.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **get_bridge_user_access_provider**
> BridgeUserAccessProviderResponse get_bridge_user_access_provider(project_id, resource_id, slug)

Fetch a bridge user-access provider by slug.

Returns the user-access provider addressed by `{slug}` on the
bridge Resource. The handler runs the `observe` ReBAC check on
the addressed Resource (`resource:<resource_id>`) BEFORE the
persistence read.


### Example


```python
import plexsphere
from plexsphere.models.bridge_user_access_provider_response import BridgeUserAccessProviderResponse
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
    api_instance = plexsphere.BridgeApi(api_client)
    project_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Project (UUIDv7). Bound on `/v1/projects/{project_id}/resources` for the Resource collection — create and list. Reused by the Bridge Orchestrator surface under `/v1/projects/{project_id}/resources/{resource_id}/bridge/...`. 
    resource_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Addressed bridge Resource (UUIDv7). Bound on every `/v1/projects/{project_id}/resources/{resource_id}/bridge/...` operation. A Resource that exists but is not kind `bridge` surfaces as `409 resource_not_bridge` on every mutating call. 
    slug = 'slug_example' # str | Stable slug of a bridge user-access provider, public-ingress rule, or site-to-site tunnel. Bound on the `/{slug}` sub-resource operations. Unique per `(resource_id, slug)` within its aggregate kind. 

    try:
        # Fetch a bridge user-access provider by slug.
        api_response = api_instance.get_bridge_user_access_provider(project_id, resource_id, slug)
        print("The response of BridgeApi->get_bridge_user_access_provider:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling BridgeApi->get_bridge_user_access_provider: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **project_id** | **UUID**| Owning Project (UUIDv7). Bound on &#x60;/v1/projects/{project_id}/resources&#x60; for the Resource collection — create and list. Reused by the Bridge Orchestrator surface under &#x60;/v1/projects/{project_id}/resources/{resource_id}/bridge/...&#x60;.  | 
 **resource_id** | **UUID**| Addressed bridge Resource (UUIDv7). Bound on every &#x60;/v1/projects/{project_id}/resources/{resource_id}/bridge/...&#x60; operation. A Resource that exists but is not kind &#x60;bridge&#x60; surfaces as &#x60;409 resource_not_bridge&#x60; on every mutating call.  | 
 **slug** | **str**| Stable slug of a bridge user-access provider, public-ingress rule, or site-to-site tunnel. Bound on the &#x60;/{slug}&#x60; sub-resource operations. Unique per &#x60;(resource_id, slug)&#x60; within its aggregate kind.  | 

### Return type

[**BridgeUserAccessProviderResponse**](BridgeUserAccessProviderResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | User-access provider found. |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to observe the addressed Resource. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | The addressed Resource or the named provider was not found. Body is a &#x60;Problem&#x60; with &#x60;code: resource_not_found&#x60;.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **list_bridge_ingress_rules**
> BridgeIngressRuleList list_bridge_ingress_rules(project_id, resource_id)

List the bridge public-ingress rules.

Returns the public-ingress rules on the addressed bridge
Resource, ordered by slug ascending. The handler runs the
`observe` ReBAC check on the addressed Resource
(`resource:<resource_id>`) BEFORE the persistence read.


### Example


```python
import plexsphere
from plexsphere.models.bridge_ingress_rule_list import BridgeIngressRuleList
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
    api_instance = plexsphere.BridgeApi(api_client)
    project_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Project (UUIDv7). Bound on `/v1/projects/{project_id}/resources` for the Resource collection — create and list. Reused by the Bridge Orchestrator surface under `/v1/projects/{project_id}/resources/{resource_id}/bridge/...`. 
    resource_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Addressed bridge Resource (UUIDv7). Bound on every `/v1/projects/{project_id}/resources/{resource_id}/bridge/...` operation. A Resource that exists but is not kind `bridge` surfaces as `409 resource_not_bridge` on every mutating call. 

    try:
        # List the bridge public-ingress rules.
        api_response = api_instance.list_bridge_ingress_rules(project_id, resource_id)
        print("The response of BridgeApi->list_bridge_ingress_rules:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling BridgeApi->list_bridge_ingress_rules: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **project_id** | **UUID**| Owning Project (UUIDv7). Bound on &#x60;/v1/projects/{project_id}/resources&#x60; for the Resource collection — create and list. Reused by the Bridge Orchestrator surface under &#x60;/v1/projects/{project_id}/resources/{resource_id}/bridge/...&#x60;.  | 
 **resource_id** | **UUID**| Addressed bridge Resource (UUIDv7). Bound on every &#x60;/v1/projects/{project_id}/resources/{resource_id}/bridge/...&#x60; operation. A Resource that exists but is not kind &#x60;bridge&#x60; surfaces as &#x60;409 resource_not_bridge&#x60; on every mutating call.  | 

### Return type

[**BridgeIngressRuleList**](BridgeIngressRuleList.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Public-ingress rules listed. |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to observe the addressed Resource. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | The addressed Resource was not found. Body is a &#x60;Problem&#x60; with &#x60;code: resource_not_found&#x60;. The bridge handler never resolves a parent Project, so &#x60;project_not_found&#x60; is not emitted on this surface.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **list_bridge_site_to_site_tunnels**
> BridgeSiteToSiteTunnelList list_bridge_site_to_site_tunnels(project_id, resource_id)

List the bridge site-to-site tunnels.

Returns the site-to-site tunnels on the addressed bridge
Resource, ordered by slug ascending. The handler runs the
`observe` ReBAC check on the addressed Resource
(`resource:<resource_id>`) BEFORE the persistence read.


### Example


```python
import plexsphere
from plexsphere.models.bridge_site_to_site_tunnel_list import BridgeSiteToSiteTunnelList
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
    api_instance = plexsphere.BridgeApi(api_client)
    project_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Project (UUIDv7). Bound on `/v1/projects/{project_id}/resources` for the Resource collection — create and list. Reused by the Bridge Orchestrator surface under `/v1/projects/{project_id}/resources/{resource_id}/bridge/...`. 
    resource_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Addressed bridge Resource (UUIDv7). Bound on every `/v1/projects/{project_id}/resources/{resource_id}/bridge/...` operation. A Resource that exists but is not kind `bridge` surfaces as `409 resource_not_bridge` on every mutating call. 

    try:
        # List the bridge site-to-site tunnels.
        api_response = api_instance.list_bridge_site_to_site_tunnels(project_id, resource_id)
        print("The response of BridgeApi->list_bridge_site_to_site_tunnels:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling BridgeApi->list_bridge_site_to_site_tunnels: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **project_id** | **UUID**| Owning Project (UUIDv7). Bound on &#x60;/v1/projects/{project_id}/resources&#x60; for the Resource collection — create and list. Reused by the Bridge Orchestrator surface under &#x60;/v1/projects/{project_id}/resources/{resource_id}/bridge/...&#x60;.  | 
 **resource_id** | **UUID**| Addressed bridge Resource (UUIDv7). Bound on every &#x60;/v1/projects/{project_id}/resources/{resource_id}/bridge/...&#x60; operation. A Resource that exists but is not kind &#x60;bridge&#x60; surfaces as &#x60;409 resource_not_bridge&#x60; on every mutating call.  | 

### Return type

[**BridgeSiteToSiteTunnelList**](BridgeSiteToSiteTunnelList.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Site-to-site tunnels listed. |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to observe the addressed Resource. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | The addressed Resource was not found. Body is a &#x60;Problem&#x60; with &#x60;code: resource_not_found&#x60;. The bridge handler never resolves a parent Project, so &#x60;project_not_found&#x60; is not emitted on this surface.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **list_bridge_user_access_providers**
> BridgeUserAccessProviderList list_bridge_user_access_providers(project_id, resource_id)

List the bridge user-access providers.

Returns the user-access providers on the addressed bridge
Resource, ordered by slug ascending. The handler runs the
`observe` ReBAC check on the addressed Resource
(`resource:<resource_id>`) BEFORE the persistence read.


### Example


```python
import plexsphere
from plexsphere.models.bridge_user_access_provider_list import BridgeUserAccessProviderList
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
    api_instance = plexsphere.BridgeApi(api_client)
    project_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Project (UUIDv7). Bound on `/v1/projects/{project_id}/resources` for the Resource collection — create and list. Reused by the Bridge Orchestrator surface under `/v1/projects/{project_id}/resources/{resource_id}/bridge/...`. 
    resource_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Addressed bridge Resource (UUIDv7). Bound on every `/v1/projects/{project_id}/resources/{resource_id}/bridge/...` operation. A Resource that exists but is not kind `bridge` surfaces as `409 resource_not_bridge` on every mutating call. 

    try:
        # List the bridge user-access providers.
        api_response = api_instance.list_bridge_user_access_providers(project_id, resource_id)
        print("The response of BridgeApi->list_bridge_user_access_providers:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling BridgeApi->list_bridge_user_access_providers: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **project_id** | **UUID**| Owning Project (UUIDv7). Bound on &#x60;/v1/projects/{project_id}/resources&#x60; for the Resource collection — create and list. Reused by the Bridge Orchestrator surface under &#x60;/v1/projects/{project_id}/resources/{resource_id}/bridge/...&#x60;.  | 
 **resource_id** | **UUID**| Addressed bridge Resource (UUIDv7). Bound on every &#x60;/v1/projects/{project_id}/resources/{resource_id}/bridge/...&#x60; operation. A Resource that exists but is not kind &#x60;bridge&#x60; surfaces as &#x60;409 resource_not_bridge&#x60; on every mutating call.  | 

### Return type

[**BridgeUserAccessProviderList**](BridgeUserAccessProviderList.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | User-access providers listed. |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to observe the addressed Resource. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | The addressed Resource was not found. Body is a &#x60;Problem&#x60; with &#x60;code: resource_not_found&#x60;. The bridge handler never resolves a parent Project, so &#x60;project_not_found&#x60; is not emitted on this surface.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **update_bridge_ingress_rule**
> BridgeIngressRuleResponse update_bridge_ingress_rule(project_id, resource_id, slug, bridge_ingress_rule_update_request)

Replace a bridge public-ingress rule's configuration.

Replaces the mutable configuration of the public-ingress rule
addressed by `{slug}`. The slug is the path identity and is not
carried in the body. The handler runs the `manage` ReBAC check
on the addressed Resource BEFORE any persistence write and
refuses a non-bridge Resource with `409 resource_not_bridge`.


### Example


```python
import plexsphere
from plexsphere.models.bridge_ingress_rule_response import BridgeIngressRuleResponse
from plexsphere.models.bridge_ingress_rule_update_request import BridgeIngressRuleUpdateRequest
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
    api_instance = plexsphere.BridgeApi(api_client)
    project_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Project (UUIDv7). Bound on `/v1/projects/{project_id}/resources` for the Resource collection — create and list. Reused by the Bridge Orchestrator surface under `/v1/projects/{project_id}/resources/{resource_id}/bridge/...`. 
    resource_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Addressed bridge Resource (UUIDv7). Bound on every `/v1/projects/{project_id}/resources/{resource_id}/bridge/...` operation. A Resource that exists but is not kind `bridge` surfaces as `409 resource_not_bridge` on every mutating call. 
    slug = 'slug_example' # str | Stable slug of a bridge user-access provider, public-ingress rule, or site-to-site tunnel. Bound on the `/{slug}` sub-resource operations. Unique per `(resource_id, slug)` within its aggregate kind. 
    bridge_ingress_rule_update_request = plexsphere.BridgeIngressRuleUpdateRequest() # BridgeIngressRuleUpdateRequest | 

    try:
        # Replace a bridge public-ingress rule's configuration.
        api_response = api_instance.update_bridge_ingress_rule(project_id, resource_id, slug, bridge_ingress_rule_update_request)
        print("The response of BridgeApi->update_bridge_ingress_rule:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling BridgeApi->update_bridge_ingress_rule: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **project_id** | **UUID**| Owning Project (UUIDv7). Bound on &#x60;/v1/projects/{project_id}/resources&#x60; for the Resource collection — create and list. Reused by the Bridge Orchestrator surface under &#x60;/v1/projects/{project_id}/resources/{resource_id}/bridge/...&#x60;.  | 
 **resource_id** | **UUID**| Addressed bridge Resource (UUIDv7). Bound on every &#x60;/v1/projects/{project_id}/resources/{resource_id}/bridge/...&#x60; operation. A Resource that exists but is not kind &#x60;bridge&#x60; surfaces as &#x60;409 resource_not_bridge&#x60; on every mutating call.  | 
 **slug** | **str**| Stable slug of a bridge user-access provider, public-ingress rule, or site-to-site tunnel. Bound on the &#x60;/{slug}&#x60; sub-resource operations. Unique per &#x60;(resource_id, slug)&#x60; within its aggregate kind.  | 
 **bridge_ingress_rule_update_request** | [**BridgeIngressRuleUpdateRequest**](BridgeIngressRuleUpdateRequest.md)|  | 

### Return type

[**BridgeIngressRuleResponse**](BridgeIngressRuleResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Public-ingress rule updated. |  -  |
**400** | Body rejected. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;relay_port_out_of_range&#x60;, &#x60;target_node_not_in_domain&#x60;, &#x60;invalid_body&#x60; }.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to manage the addressed Resource. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | The addressed Resource or the named rule was not found. Body is a &#x60;Problem&#x60; with &#x60;code: resource_not_found&#x60;.  |  -  |
**409** | Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;resource_not_bridge&#x60; (the addressed Resource is not kind &#x60;bridge&#x60;), &#x60;slug_conflict&#x60; (the patched &#x60;sni_host&#x60; collides with another rule on this Resource), &#x60;bridge_port_conflict&#x60; (the patched &#x60;(target_node_id, target_port)&#x60; pair is already claimed by a sibling ingress rule within the bridge — refused by the cross-aggregate validator before persistence) }.  |  -  |
**413** | Request body exceeded the bridge body ceiling.  |  -  |
**422** | The patched &#x60;acme_account_ref&#x60; names an ACME directory that is unreachable, returned a server error, timed out, or advertised a malformed directory document. Body is a &#x60;Problem&#x60; with &#x60;code: acme_directory_unreachable&#x60;. The request is well-formed but the certificate issuance the rule requires cannot be provisioned, so the cross-aggregate validator refuses it before persistence.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **update_bridge_site_to_site_tunnel**
> BridgeSiteToSiteTunnelResponse update_bridge_site_to_site_tunnel(project_id, resource_id, slug, bridge_site_to_site_tunnel_update_request)

Replace a bridge site-to-site tunnel's configuration.

Replaces the mutable configuration of the site-to-site tunnel
addressed by `{slug}`. The slug is the path identity and is not
carried in the body. The handler runs the `manage` ReBAC check
on the addressed Resource BEFORE any persistence write and
refuses a non-bridge Resource with `409 resource_not_bridge`.


### Example


```python
import plexsphere
from plexsphere.models.bridge_site_to_site_tunnel_response import BridgeSiteToSiteTunnelResponse
from plexsphere.models.bridge_site_to_site_tunnel_update_request import BridgeSiteToSiteTunnelUpdateRequest
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
    api_instance = plexsphere.BridgeApi(api_client)
    project_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Project (UUIDv7). Bound on `/v1/projects/{project_id}/resources` for the Resource collection — create and list. Reused by the Bridge Orchestrator surface under `/v1/projects/{project_id}/resources/{resource_id}/bridge/...`. 
    resource_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Addressed bridge Resource (UUIDv7). Bound on every `/v1/projects/{project_id}/resources/{resource_id}/bridge/...` operation. A Resource that exists but is not kind `bridge` surfaces as `409 resource_not_bridge` on every mutating call. 
    slug = 'slug_example' # str | Stable slug of a bridge user-access provider, public-ingress rule, or site-to-site tunnel. Bound on the `/{slug}` sub-resource operations. Unique per `(resource_id, slug)` within its aggregate kind. 
    bridge_site_to_site_tunnel_update_request = plexsphere.BridgeSiteToSiteTunnelUpdateRequest() # BridgeSiteToSiteTunnelUpdateRequest | 

    try:
        # Replace a bridge site-to-site tunnel's configuration.
        api_response = api_instance.update_bridge_site_to_site_tunnel(project_id, resource_id, slug, bridge_site_to_site_tunnel_update_request)
        print("The response of BridgeApi->update_bridge_site_to_site_tunnel:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling BridgeApi->update_bridge_site_to_site_tunnel: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **project_id** | **UUID**| Owning Project (UUIDv7). Bound on &#x60;/v1/projects/{project_id}/resources&#x60; for the Resource collection — create and list. Reused by the Bridge Orchestrator surface under &#x60;/v1/projects/{project_id}/resources/{resource_id}/bridge/...&#x60;.  | 
 **resource_id** | **UUID**| Addressed bridge Resource (UUIDv7). Bound on every &#x60;/v1/projects/{project_id}/resources/{resource_id}/bridge/...&#x60; operation. A Resource that exists but is not kind &#x60;bridge&#x60; surfaces as &#x60;409 resource_not_bridge&#x60; on every mutating call.  | 
 **slug** | **str**| Stable slug of a bridge user-access provider, public-ingress rule, or site-to-site tunnel. Bound on the &#x60;/{slug}&#x60; sub-resource operations. Unique per &#x60;(resource_id, slug)&#x60; within its aggregate kind.  | 
 **bridge_site_to_site_tunnel_update_request** | [**BridgeSiteToSiteTunnelUpdateRequest**](BridgeSiteToSiteTunnelUpdateRequest.md)|  | 

### Return type

[**BridgeSiteToSiteTunnelResponse**](BridgeSiteToSiteTunnelResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Site-to-site tunnel updated. |  -  |
**400** | Body rejected. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;relay_port_out_of_range&#x60;, &#x60;allowed_subnet_empty&#x60;, &#x60;secret_ref_malformed&#x60;, &#x60;invalid_body&#x60; }.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to manage the addressed Resource. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | The addressed Resource or the named tunnel was not found. Body is a &#x60;Problem&#x60; with &#x60;code: resource_not_found&#x60;.  |  -  |
**409** | Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;resource_not_bridge&#x60; (the addressed Resource is not kind &#x60;bridge&#x60;), &#x60;subnet_overlap_with_mesh&#x60; (a patched &#x60;allowed_subnets&#x60; entry overlaps the Domain mesh CIDR), &#x60;subnet_overlap_with_tunnel&#x60; (a patched &#x60;allowed_subnets&#x60; entry overlaps a sibling tunnel within the bridge) — the two overlap codes are raised by the cross-aggregate validator before persistence, with the mesh check taking precedence over the sibling-tunnel check.  |  -  |
**413** | Request body exceeded the bridge body ceiling.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **update_bridge_user_access_provider**
> BridgeUserAccessProviderResponse update_bridge_user_access_provider(project_id, resource_id, slug, bridge_user_access_provider_update_request)

Replace a bridge user-access provider's configuration.

Replaces the mutable configuration of the user-access provider
addressed by `{slug}`. The slug is the path identity and is not
carried in the body. The handler runs the `manage` ReBAC check
on the addressed Resource BEFORE any persistence write and
refuses a non-bridge Resource with `409 resource_not_bridge`.


### Example


```python
import plexsphere
from plexsphere.models.bridge_user_access_provider_response import BridgeUserAccessProviderResponse
from plexsphere.models.bridge_user_access_provider_update_request import BridgeUserAccessProviderUpdateRequest
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
    api_instance = plexsphere.BridgeApi(api_client)
    project_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Project (UUIDv7). Bound on `/v1/projects/{project_id}/resources` for the Resource collection — create and list. Reused by the Bridge Orchestrator surface under `/v1/projects/{project_id}/resources/{resource_id}/bridge/...`. 
    resource_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Addressed bridge Resource (UUIDv7). Bound on every `/v1/projects/{project_id}/resources/{resource_id}/bridge/...` operation. A Resource that exists but is not kind `bridge` surfaces as `409 resource_not_bridge` on every mutating call. 
    slug = 'slug_example' # str | Stable slug of a bridge user-access provider, public-ingress rule, or site-to-site tunnel. Bound on the `/{slug}` sub-resource operations. Unique per `(resource_id, slug)` within its aggregate kind. 
    bridge_user_access_provider_update_request = plexsphere.BridgeUserAccessProviderUpdateRequest() # BridgeUserAccessProviderUpdateRequest | 

    try:
        # Replace a bridge user-access provider's configuration.
        api_response = api_instance.update_bridge_user_access_provider(project_id, resource_id, slug, bridge_user_access_provider_update_request)
        print("The response of BridgeApi->update_bridge_user_access_provider:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling BridgeApi->update_bridge_user_access_provider: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **project_id** | **UUID**| Owning Project (UUIDv7). Bound on &#x60;/v1/projects/{project_id}/resources&#x60; for the Resource collection — create and list. Reused by the Bridge Orchestrator surface under &#x60;/v1/projects/{project_id}/resources/{resource_id}/bridge/...&#x60;.  | 
 **resource_id** | **UUID**| Addressed bridge Resource (UUIDv7). Bound on every &#x60;/v1/projects/{project_id}/resources/{resource_id}/bridge/...&#x60; operation. A Resource that exists but is not kind &#x60;bridge&#x60; surfaces as &#x60;409 resource_not_bridge&#x60; on every mutating call.  | 
 **slug** | **str**| Stable slug of a bridge user-access provider, public-ingress rule, or site-to-site tunnel. Bound on the &#x60;/{slug}&#x60; sub-resource operations. Unique per &#x60;(resource_id, slug)&#x60; within its aggregate kind.  | 
 **bridge_user_access_provider_update_request** | [**BridgeUserAccessProviderUpdateRequest**](BridgeUserAccessProviderUpdateRequest.md)|  | 

### Return type

[**BridgeUserAccessProviderResponse**](BridgeUserAccessProviderResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | User-access provider updated. |  -  |
**400** | Body rejected. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;relay_port_out_of_range&#x60;, &#x60;secret_ref_malformed&#x60;, &#x60;invalid_body&#x60; }.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to manage the addressed Resource. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | The addressed Resource or the named provider was not found. Body is a &#x60;Problem&#x60; with &#x60;code: resource_not_found&#x60;.  |  -  |
**409** | Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;resource_not_bridge&#x60; (the addressed Resource is not kind &#x60;bridge&#x60;), &#x60;bridge_port_conflict&#x60; (the patched &#x60;listen_port&#x60; is already bound by the relay or another provider or ingress rule within the bridge — refused by the cross-aggregate validator before persistence) }.  |  -  |
**413** | Request body exceeded the bridge body ceiling.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

