# BridgeIngressRuleList

Public-ingress rules on a bridge Resource, returned by `ListBridgeIngressRules`. Ordered by slug ascending. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**rules** | [**List[BridgeIngressRuleResponse]**](BridgeIngressRuleResponse.md) | Rules ordered by slug ascending. | 

## Example

```python
from plexsphere.models.bridge_ingress_rule_list import BridgeIngressRuleList

# TODO update the JSON string below
json = "{}"
# create an instance of BridgeIngressRuleList from a JSON string
bridge_ingress_rule_list_instance = BridgeIngressRuleList.from_json(json)
# print the JSON string representation of the object
print(BridgeIngressRuleList.to_json())

# convert the object into a dict
bridge_ingress_rule_list_dict = bridge_ingress_rule_list_instance.to_dict()
# create an instance of BridgeIngressRuleList from a dict
bridge_ingress_rule_list_from_dict = BridgeIngressRuleList.from_dict(bridge_ingress_rule_list_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


