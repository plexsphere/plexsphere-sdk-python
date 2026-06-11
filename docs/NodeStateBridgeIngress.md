# NodeStateBridgeIngress

Per-Node projection of the bridge public-ingress rules the Node must program. Reuses the `BridgeIngressRuleResponse` wire shape so the delivered config matches the management-surface bytes for the same rule. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**rules** | [**List[BridgeIngressRuleResponse]**](BridgeIngressRuleResponse.md) | Public-ingress rules ordered by slug ascending. | 

## Example

```python
from plexsphere.models.node_state_bridge_ingress import NodeStateBridgeIngress

# TODO update the JSON string below
json = "{}"
# create an instance of NodeStateBridgeIngress from a JSON string
node_state_bridge_ingress_instance = NodeStateBridgeIngress.from_json(json)
# print the JSON string representation of the object
print(NodeStateBridgeIngress.to_json())

# convert the object into a dict
node_state_bridge_ingress_dict = node_state_bridge_ingress_instance.to_dict()
# create an instance of NodeStateBridgeIngress from a dict
node_state_bridge_ingress_from_dict = NodeStateBridgeIngress.from_dict(node_state_bridge_ingress_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


