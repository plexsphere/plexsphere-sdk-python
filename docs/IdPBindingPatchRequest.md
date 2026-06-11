# IdPBindingPatchRequest

Partial-update body for PATCH /v1/admin/idp/{id}. All fields are optional; only fields present in the request are mutated. the `status` field is intentionally absent from this schema — status transitions are performed exclusively through PATCH /v1/admin/idp/{id}/status. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**discovery_url** | **str** | OIDC discovery document URL (absolute http/https). | [optional] 
**claim_mappings** | **Dict[str, str]** | Mapping of plexsphere claim name → IdP claim name. An empty map clears all mappings.  | [optional] 
**required_acr** | **List[str]** | Required OIDC ACR values. | [optional] 
**required_amr** | **List[str]** | Required OIDC AMR values. | [optional] 
**jit_policy** | **str** | Just-in-time user-provisioning policy. | [optional] 

## Example

```python
from plexsphere.models.id_p_binding_patch_request import IdPBindingPatchRequest

# TODO update the JSON string below
json = "{}"
# create an instance of IdPBindingPatchRequest from a JSON string
id_p_binding_patch_request_instance = IdPBindingPatchRequest.from_json(json)
# print the JSON string representation of the object
print(IdPBindingPatchRequest.to_json())

# convert the object into a dict
id_p_binding_patch_request_dict = id_p_binding_patch_request_instance.to_dict()
# create an instance of IdPBindingPatchRequest from a dict
id_p_binding_patch_request_from_dict = IdPBindingPatchRequest.from_dict(id_p_binding_patch_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


