# IdPBindingResponse

Response echo of a persisted IdPBinding aggregate. The `client_secret_ref` field is included because it is an opaque reference, not the secret itself. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**id** | **UUID** | Binding identifier (UUIDv7). | 
**domain_id** | **UUID** | Owning Domain, or null for a platform-scoped (shared) binding usable by any Domain.  | [optional] 
**issuer** | **str** | OIDC issuer URL. | 
**client_id** | **str** | OIDC client identifier. | 
**client_secret_ref** | **str** | Opaque reference to the client secret. | 
**discovery_url** | **str** | OIDC discovery document URL. | 
**claim_mappings** | **Dict[str, str]** | Plexsphere claim → IdP claim mapping. | [optional] 
**required_acr** | **List[str]** |  | [optional] 
**required_amr** | **List[str]** |  | [optional] 
**jit_policy** | **str** |  | 
**status** | **str** | Aggregate status. | 
**alias** | **str** | Human-friendly handle, unique per Domain among active bindings. Absent when the binding has no alias.  | [optional] 
**primary** | **bool** | Whether this binding is the Domain default a Domain-only login resolves to.  | 
**created_at** | **datetime** |  | 
**updated_at** | **datetime** |  | 

## Example

```python
from plexsphere.models.id_p_binding_response import IdPBindingResponse

# TODO update the JSON string below
json = "{}"
# create an instance of IdPBindingResponse from a JSON string
id_p_binding_response_instance = IdPBindingResponse.from_json(json)
# print the JSON string representation of the object
print(IdPBindingResponse.to_json())

# convert the object into a dict
id_p_binding_response_dict = id_p_binding_response_instance.to_dict()
# create an instance of IdPBindingResponse from a dict
id_p_binding_response_from_dict = IdPBindingResponse.from_dict(id_p_binding_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


