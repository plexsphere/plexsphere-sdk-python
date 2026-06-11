# NodeStateBridgeRelayAssignment

One per-peer relay fallback assignment: the peer the Node may relay for and the fallback endpoint that peer dials. Population lands with the effective-config story; an empty `assignments` list is the interim state. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**peer_node_id** | **UUID** | Peer Node the relay fallback applies to. | 
**fallback_endpoint** | **str** | Host the peer dials when direct connectivity is unavailable. | 
**fallback_endpoint_port** | **int** | Port on the fallback endpoint the peer dials. | 

## Example

```python
from plexsphere.models.node_state_bridge_relay_assignment import NodeStateBridgeRelayAssignment

# TODO update the JSON string below
json = "{}"
# create an instance of NodeStateBridgeRelayAssignment from a JSON string
node_state_bridge_relay_assignment_instance = NodeStateBridgeRelayAssignment.from_json(json)
# print the JSON string representation of the object
print(NodeStateBridgeRelayAssignment.to_json())

# convert the object into a dict
node_state_bridge_relay_assignment_dict = node_state_bridge_relay_assignment_instance.to_dict()
# create an instance of NodeStateBridgeRelayAssignment from a dict
node_state_bridge_relay_assignment_from_dict = NodeStateBridgeRelayAssignment.from_dict(node_state_bridge_relay_assignment_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


