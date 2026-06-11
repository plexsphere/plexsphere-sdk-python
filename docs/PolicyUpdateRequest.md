# PolicyUpdateRequest

Body for patching a Policy. Any field set lands a new revision carrying the updated values; unset fields inherit from the current head revision. `expected_revision_id` enables optimistic concurrency — a stale value loses the partial-unique-head race and surfaces as `409 revision_conflict`, giving the caller the opportunity to re-fetch the current head and rebase the edit. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**display_name** | **str** |  | [optional] 
**selector** | [**PolicySelector**](PolicySelector.md) |  | [optional] 
**rules** | [**List[PolicyRule]**](PolicyRule.md) |  | [optional] 
**expected_revision_id** | **UUID** | Revision identifier the client believes is current. When present, the PATCH only succeeds if the persisted head still matches; otherwise the call surfaces as &#x60;409 revision_conflict&#x60;.  | [optional] 

## Example

```python
from plexsphere.models.policy_update_request import PolicyUpdateRequest

# TODO update the JSON string below
json = "{}"
# create an instance of PolicyUpdateRequest from a JSON string
policy_update_request_instance = PolicyUpdateRequest.from_json(json)
# print the JSON string representation of the object
print(PolicyUpdateRequest.to_json())

# convert the object into a dict
policy_update_request_dict = policy_update_request_instance.to_dict()
# create an instance of PolicyUpdateRequest from a dict
policy_update_request_from_dict = PolicyUpdateRequest.from_dict(policy_update_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


