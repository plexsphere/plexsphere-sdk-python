# PlatformIdPBindingRequest

Body for POST /v1/admin/platform-idp. Registers a platform-scoped (shared) IdP binding owned by no Domain and usable by any Domain. The field set mirrors IdPBindingRequest minus `domain_id` (a shared binding has no owning Domain) and `primary` (a shared binding has no Domain to be the default for). 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**issuer** | **str** | OIDC issuer URL (absolute http/https). | 
**client_id** | **str** | OIDC client identifier. | 
**client_secret_ref** | **str** | Opaque reference to the client secret held in the secret store — never the secret value itself.  | 
**discovery_url** | **str** | OIDC discovery document URL (absolute http/https). | 
**claim_mappings** | **Dict[str, str]** | Mapping of plexsphere claim name → IdP claim name. An empty map is permitted. The mappings apply uniformly to every Domain that resolves through this shared binding.  | [optional] 
**required_acr** | **List[str]** | Required OIDC ACR values. | [optional] 
**required_amr** | **List[str]** | Required OIDC AMR values. | [optional] 
**jit_policy** | **str** | Just-in-time user-provisioning policy. One of &#x60;allow&#x60; or &#x60;deny&#x60;; the server rejects any other value with 400 &#x60;invalid-jit-policy&#x60;. Declared as a plain string rather than an inline enum so it shares the IdPBinding jit-policy vocabulary without minting a fourth generated enum type.  | 
**alias** | **str** | Optional human-friendly handle for the binding, unique among active platform bindings (e.g. &#x60;github&#x60;). Normalised to lowercase kebab-case, so an operator may submit mixed case.  | [optional] 

## Example

```python
from plexsphere.models.platform_id_p_binding_request import PlatformIdPBindingRequest

# TODO update the JSON string below
json = "{}"
# create an instance of PlatformIdPBindingRequest from a JSON string
platform_id_p_binding_request_instance = PlatformIdPBindingRequest.from_json(json)
# print the JSON string representation of the object
print(PlatformIdPBindingRequest.to_json())

# convert the object into a dict
platform_id_p_binding_request_dict = platform_id_p_binding_request_instance.to_dict()
# create an instance of PlatformIdPBindingRequest from a dict
platform_id_p_binding_request_from_dict = PlatformIdPBindingRequest.from_dict(platform_id_p_binding_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


