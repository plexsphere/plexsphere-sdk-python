# NodeStateCompiledRule

Single canonicalised L3/L4 rule the controller projected for the addressed Node. The five-tuple `(source_cidr, destination_cidr, protocol, ports, action)` mirrors `PolicyRule` so plexd's reconcile loop can reuse the same rule programmer regardless of whether the rule came from the operator-facing Policy aggregate or the compiled fan-out. `ports` MUST be omitted for `icmp` and `any` and MUST be populated for `tcp` and `udp` — the protocol/port coherence check is enforced at the policy aggregate boundary, so a compiled rule that reaches the wire is guaranteed coherent. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**action** | [**PolicyAction**](PolicyAction.md) |  | 
**protocol** | [**PolicyProtocol**](PolicyProtocol.md) |  | 
**source_cidr** | **str** | IPv4 or IPv6 prefix in CIDR notation (e.g. &#x60;10.0.0.0/8&#x60;, &#x60;2001:db8::/32&#x60;). Source and destination agree on address family by aggregate-boundary invariant.  | 
**destination_cidr** | **str** | IPv4 or IPv6 prefix in CIDR notation. Address family matches &#x60;source_cidr&#x60;.  | 
**ports** | [**PolicyPortRange**](PolicyPortRange.md) |  | [optional] 

## Example

```python
from plexsphere.models.node_state_compiled_rule import NodeStateCompiledRule

# TODO update the JSON string below
json = "{}"
# create an instance of NodeStateCompiledRule from a JSON string
node_state_compiled_rule_instance = NodeStateCompiledRule.from_json(json)
# print the JSON string representation of the object
print(NodeStateCompiledRule.to_json())

# convert the object into a dict
node_state_compiled_rule_dict = node_state_compiled_rule_instance.to_dict()
# create an instance of NodeStateCompiledRule from a dict
node_state_compiled_rule_from_dict = NodeStateCompiledRule.from_dict(node_state_compiled_rule_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


