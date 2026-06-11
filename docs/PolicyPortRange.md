# PolicyPortRange

Inclusive `[from, to]` L4 port range. Required when the sibling `protocol` is `tcp` or `udp`; MUST be omitted for `icmp` and `any`. `from` MUST be ≤ `to` — an inverted range surfaces as `422 port_range_inverted`. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**var_from** | **int** |  | 
**to** | **int** |  | 

## Example

```python
from plexsphere.models.policy_port_range import PolicyPortRange

# TODO update the JSON string below
json = "{}"
# create an instance of PolicyPortRange from a JSON string
policy_port_range_instance = PolicyPortRange.from_json(json)
# print the JSON string representation of the object
print(PolicyPortRange.to_json())

# convert the object into a dict
policy_port_range_dict = policy_port_range_instance.to_dict()
# create an instance of PolicyPortRange from a dict
policy_port_range_from_dict = PolicyPortRange.from_dict(policy_port_range_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


