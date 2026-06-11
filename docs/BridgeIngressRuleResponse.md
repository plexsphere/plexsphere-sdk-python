# BridgeIngressRuleResponse

Hydrated projection of a bridge public-ingress rule. Shared by `CreateBridgeIngressRule`, `GetBridgeIngressRule`, `UpdateBridgeIngressRule`, and the list wrapper. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**id** | **UUID** | Rule identifier (UUIDv7). | 
**slug** | **str** | Stable identity of the rule within its Resource. | 
**sni_host** | **str** | TLS SNI host the rule terminates. | 
**target_node_id** | **UUID** | Node the rule forwards to. | 
**target_port** | **int** | TCP port on the target Node. | 
**acme_account_ref** | **str** | Opaque reference to the ACME account used to issue the rule&#39;s certificate. &#x60;null&#x60; when the operator supplies certificates out of band.  | [optional] 
**created_at** | **datetime** | Rule creation timestamp (UTC). | 
**updated_at** | **datetime** | Last-modified timestamp (UTC). | 

## Example

```python
from plexsphere.models.bridge_ingress_rule_response import BridgeIngressRuleResponse

# TODO update the JSON string below
json = "{}"
# create an instance of BridgeIngressRuleResponse from a JSON string
bridge_ingress_rule_response_instance = BridgeIngressRuleResponse.from_json(json)
# print the JSON string representation of the object
print(BridgeIngressRuleResponse.to_json())

# convert the object into a dict
bridge_ingress_rule_response_dict = bridge_ingress_rule_response_instance.to_dict()
# create an instance of BridgeIngressRuleResponse from a dict
bridge_ingress_rule_response_from_dict = BridgeIngressRuleResponse.from_dict(bridge_ingress_rule_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


