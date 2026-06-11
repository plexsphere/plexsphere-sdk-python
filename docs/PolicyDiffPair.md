# PolicyDiffPair

Before / after pair for a rule whose action flipped between two revisions. The pair shares a stable rule key `(source_cidr, destination_cidr, protocol, ports)`; only `action` differs. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**before** | [**PolicyRule**](PolicyRule.md) |  | 
**after** | [**PolicyRule**](PolicyRule.md) |  | 

## Example

```python
from plexsphere.models.policy_diff_pair import PolicyDiffPair

# TODO update the JSON string below
json = "{}"
# create an instance of PolicyDiffPair from a JSON string
policy_diff_pair_instance = PolicyDiffPair.from_json(json)
# print the JSON string representation of the object
print(PolicyDiffPair.to_json())

# convert the object into a dict
policy_diff_pair_dict = policy_diff_pair_instance.to_dict()
# create an instance of PolicyDiffPair from a dict
policy_diff_pair_from_dict = PolicyDiffPair.from_dict(policy_diff_pair_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


