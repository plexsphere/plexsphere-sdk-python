# NodeStateBridge

Per-Node bridge-orchestrator delivered-config block. Carries the relay configuration, the user-access providers, the public ingress rules, and the site-to-site tunnels the addressed Node must program. Each child block is present-but-nullable so plexd's reconcile loop can diff by field presence. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**relay** | [**NodeStateBridgeRelay**](NodeStateBridgeRelay.md) | Relay configuration the Node must program. &#x60;null&#x60; when no relay is configured on the owning bridge Resource.  | [optional] 
**user_access** | [**NodeStateBridgeUserAccess**](NodeStateBridgeUserAccess.md) | User-access providers the Node must program. &#x60;null&#x60; when the owning bridge Resource defines none.  | [optional] 
**ingress** | [**NodeStateBridgeIngress**](NodeStateBridgeIngress.md) | Public-ingress rules the Node must program. &#x60;null&#x60; when the owning bridge Resource defines none.  | [optional] 
**site_to_site** | [**NodeStateBridgeSiteToSite**](NodeStateBridgeSiteToSite.md) | Site-to-site tunnels the Node must program. &#x60;null&#x60; when the owning bridge Resource defines none.  | [optional] 

## Example

```python
from plexsphere.models.node_state_bridge import NodeStateBridge

# TODO update the JSON string below
json = "{}"
# create an instance of NodeStateBridge from a JSON string
node_state_bridge_instance = NodeStateBridge.from_json(json)
# print the JSON string representation of the object
print(NodeStateBridge.to_json())

# convert the object into a dict
node_state_bridge_dict = node_state_bridge_instance.to_dict()
# create an instance of NodeStateBridge from a dict
node_state_bridge_from_dict = NodeStateBridge.from_dict(node_state_bridge_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


