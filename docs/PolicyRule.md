# PolicyRule

Single L3/L4 rule on a Policy revision. The five-tuple `(source_cidr, destination_cidr, protocol, ports, action)` describes which packets the rule governs and what verdict to apply. `ports` MUST be omitted for `icmp` and `any` and MUST be populated for `tcp` and `udp` — the protocol/port coherence check is enforced at the aggregate boundary. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**action** | [**PolicyAction**](PolicyAction.md) |  | 
**protocol** | [**PolicyProtocol**](PolicyProtocol.md) |  | 
**source_cidr** | **str** | IPv4 or IPv6 prefix in CIDR notation (e.g. &#x60;10.0.0.0/8&#x60;, &#x60;2001:db8::/32&#x60;). Source and destination MUST agree on address family; a mismatch surfaces as &#x60;422 cidr_family_mismatch&#x60;.  | 
**destination_cidr** | **str** | IPv4 or IPv6 prefix in CIDR notation. Address family MUST match &#x60;source_cidr&#x60;.  | 
**ports** | [**PolicyPortRange**](PolicyPortRange.md) |  | [optional] 

## Example

```python
from plexsphere.models.policy_rule import PolicyRule

# TODO update the JSON string below
json = "{}"
# create an instance of PolicyRule from a JSON string
policy_rule_instance = PolicyRule.from_json(json)
# print the JSON string representation of the object
print(PolicyRule.to_json())

# convert the object into a dict
policy_rule_dict = policy_rule_instance.to_dict()
# create an instance of PolicyRule from a dict
policy_rule_from_dict = PolicyRule.from_dict(policy_rule_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


