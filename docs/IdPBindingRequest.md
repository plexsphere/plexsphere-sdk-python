# IdPBindingRequest

Body for POST /v1/admin/idp. Field set mirrors the `IdPBinding` aggregate. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**domain_id** | **UUID** | Owning Domain. | 
**issuer** | **str** | OIDC issuer URL (absolute http/https). | 
**client_id** | **str** | OIDC client identifier. | 
**client_secret_ref** | **str** | Opaque reference to the client secret held in the secret store — never the secret value itself.  | 
**discovery_url** | **str** | OIDC discovery document URL (absolute http/https). | 
**claim_mappings** | **Dict[str, str]** | Mapping of plexsphere claim name → IdP claim name. An empty map is permitted.  | [optional] 
**required_acr** | **List[str]** | Required OIDC ACR values. | [optional] 
**required_amr** | **List[str]** | Required OIDC AMR values. | [optional] 
**jit_policy** | **str** | Just-in-time user-provisioning policy. | 
**alias** | **str** | Optional human-friendly handle for the binding, unique per Domain among active bindings (e.g. &#x60;github&#x60;). Normalised to lowercase kebab-case, so an operator may submit mixed case.  | [optional] 
**primary** | **bool** | Mark this binding as the Domain default a Domain-only login resolves to. At most one active primary per Domain; a second primary is rejected with 409 primary-conflict.  | [optional] 

## Example

```python
from plexsphere.models.id_p_binding_request import IdPBindingRequest

# TODO update the JSON string below
json = "{}"
# create an instance of IdPBindingRequest from a JSON string
id_p_binding_request_instance = IdPBindingRequest.from_json(json)
# print the JSON string representation of the object
print(IdPBindingRequest.to_json())

# convert the object into a dict
id_p_binding_request_dict = id_p_binding_request_instance.to_dict()
# create an instance of IdPBindingRequest from a dict
id_p_binding_request_from_dict = IdPBindingRequest.from_dict(id_p_binding_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


