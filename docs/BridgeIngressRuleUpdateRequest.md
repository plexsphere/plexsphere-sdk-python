# BridgeIngressRuleUpdateRequest

Body for `UpdateBridgeIngressRule`. Full replacement of the rule's mutable configuration — not a partial merge. The slug is the path identity and is not carried in the body. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**sni_host** | **str** | TLS SNI host the rule terminates. Unique per &#x60;(resource_id, sni_host)&#x60;; a collision surfaces as &#x60;409 slug_conflict&#x60;.  | 
**target_node_id** | **UUID** | Node the rule forwards to. A Node outside the bridge Resource&#39;s owning Domain surfaces as &#x60;400 target_node_not_in_domain&#x60;.  | 
**target_port** | **int** | TCP port on the target Node. Outside &#x60;1..65535&#x60; the write is rejected with &#x60;400 relay_port_out_of_range&#x60;.  | 
**acme_account_ref** | **str** | Optional opaque reference to the ACME account used to issue the rule&#39;s certificate. &#x60;null&#x60; or absent means the operator supplies certificates out of band.  | [optional] 

## Example

```python
from plexsphere.models.bridge_ingress_rule_update_request import BridgeIngressRuleUpdateRequest

# TODO update the JSON string below
json = "{}"
# create an instance of BridgeIngressRuleUpdateRequest from a JSON string
bridge_ingress_rule_update_request_instance = BridgeIngressRuleUpdateRequest.from_json(json)
# print the JSON string representation of the object
print(BridgeIngressRuleUpdateRequest.to_json())

# convert the object into a dict
bridge_ingress_rule_update_request_dict = bridge_ingress_rule_update_request_instance.to_dict()
# create an instance of BridgeIngressRuleUpdateRequest from a dict
bridge_ingress_rule_update_request_from_dict = BridgeIngressRuleUpdateRequest.from_dict(bridge_ingress_rule_update_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


