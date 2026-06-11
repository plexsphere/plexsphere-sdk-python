# GroupResponse

Response echo of a persisted Group aggregate. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**id** | **UUID** | Group identifier (UUIDv7). | 
**domain_id** | **UUID** | Owning Domain. | 
**slug** | **str** | URL-safe identifier unique within &#x60;domain_id&#x60; .  | 
**display_name** | **str** | Human-friendly Group name. | 
**source** | **str** | Provenance of the Group. | 
**idp_binding_id** | **UUID** | IdP binding the Group is reconciled from. Present only when &#x60;source&#x3D;idp&#x60;.  | [optional] 
**idp_claim_value** | **str** | Verbatim IdP claim value. Present only when &#x60;source&#x3D;idp&#x60; .  | [optional] 
**created_at** | **datetime** |  | 
**updated_at** | **datetime** |  | 

## Example

```python
from plexsphere.models.group_response import GroupResponse

# TODO update the JSON string below
json = "{}"
# create an instance of GroupResponse from a JSON string
group_response_instance = GroupResponse.from_json(json)
# print the JSON string representation of the object
print(GroupResponse.to_json())

# convert the object into a dict
group_response_dict = group_response_instance.to_dict()
# create an instance of GroupResponse from a dict
group_response_from_dict = GroupResponse.from_dict(group_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


