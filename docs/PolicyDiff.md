# PolicyDiff

Structural difference between two canonicalised Policy rule lists. Canonicalisation sorts the list into a deterministic order so a reorder-only edit produces empty `added`, `removed`, and `modified` arrays. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**added** | [**List[PolicyRule]**](PolicyRule.md) | Rules present in the target revision but not the source. | 
**removed** | [**List[PolicyRule]**](PolicyRule.md) | Rules present in the source revision but not the target. | 
**modified** | [**List[PolicyDiffPair]**](PolicyDiffPair.md) | Rules sharing a stable key on both sides where only the &#x60;action&#x60; field differs.  | 

## Example

```python
from plexsphere.models.policy_diff import PolicyDiff

# TODO update the JSON string below
json = "{}"
# create an instance of PolicyDiff from a JSON string
policy_diff_instance = PolicyDiff.from_json(json)
# print the JSON string representation of the object
print(PolicyDiff.to_json())

# convert the object into a dict
policy_diff_dict = policy_diff_instance.to_dict()
# create an instance of PolicyDiff from a dict
policy_diff_from_dict = PolicyDiff.from_dict(policy_diff_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


