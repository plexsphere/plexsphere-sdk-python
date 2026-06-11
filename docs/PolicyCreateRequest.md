# PolicyCreateRequest

Body for creating a Policy. Lands the Policy aggregate row and its initial revision in one transaction. The initial revision carries the supplied `selector` and `rules`; the head pointer is set to the new revision's identifier. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**slug** | **str** |  | 
**display_name** | **str** |  | [optional] 
**selector** | [**PolicySelector**](PolicySelector.md) |  | 
**rules** | [**List[PolicyRule]**](PolicyRule.md) |  | 

## Example

```python
from plexsphere.models.policy_create_request import PolicyCreateRequest

# TODO update the JSON string below
json = "{}"
# create an instance of PolicyCreateRequest from a JSON string
policy_create_request_instance = PolicyCreateRequest.from_json(json)
# print the JSON string representation of the object
print(PolicyCreateRequest.to_json())

# convert the object into a dict
policy_create_request_dict = policy_create_request_instance.to_dict()
# create an instance of PolicyCreateRequest from a dict
policy_create_request_from_dict = PolicyCreateRequest.from_dict(policy_create_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


