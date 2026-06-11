# NodeStateBridgeRelay

Per-Node projection of the bridge relay configuration paired with the per-peer fallback-endpoint assignments the Node must program. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**enabled** | **bool** | Whether the relay is active. | 
**listen_port** | **int** | UDP port the relay listens on. | 
**assignments** | [**List[NodeStateBridgeRelayAssignment]**](NodeStateBridgeRelayAssignment.md) | Per-peer relay fallback-endpoint assignments, ordered deterministically by &#x60;peer_node_id&#x60; so two consecutive pulls against the same ledger snapshot are byte-equal. An empty array is a legitimate state.  | 

## Example

```python
from plexsphere.models.node_state_bridge_relay import NodeStateBridgeRelay

# TODO update the JSON string below
json = "{}"
# create an instance of NodeStateBridgeRelay from a JSON string
node_state_bridge_relay_instance = NodeStateBridgeRelay.from_json(json)
# print the JSON string representation of the object
print(NodeStateBridgeRelay.to_json())

# convert the object into a dict
node_state_bridge_relay_dict = node_state_bridge_relay_instance.to_dict()
# create an instance of NodeStateBridgeRelay from a dict
node_state_bridge_relay_from_dict = NodeStateBridgeRelay.from_dict(node_state_bridge_relay_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


