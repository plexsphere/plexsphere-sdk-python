# NodeStatePeer

Minimal projection of a Domain peer that the addressed Node needs in order to program its WireGuard table. The shape is deliberately narrow and mirrors `RegisterPeer` so the reconciliation pull and the registration bootstrap converge on the same wire contract. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**node_id** | **UUID** | Peer&#39;s Node identifier. Plexd&#39;s reconcile loop keys peers by id.  | 
**mesh_ip** | **str** | Peer&#39;s allocator-assigned mesh address. Plexd programs the WireGuard endpoint from this field.  | 
**public_key** | **str** | Peer&#39;s WireGuard public key, base64-encoded with standard padding.  | 
**fallback_endpoint** | **str** | Canonical bridge &#x60;host:port&#x60; that plexd programs as a NAT-traversal relay when direct peer-to-peer connectivity is unavailable. Empty or absent means no bridge is currently assigned for this peer. Format: &#x60;&lt;inet-or-hostname&gt;:&lt;port&gt;&#x60;.  | [optional] 

## Example

```python
from plexsphere.models.node_state_peer import NodeStatePeer

# TODO update the JSON string below
json = "{}"
# create an instance of NodeStatePeer from a JSON string
node_state_peer_instance = NodeStatePeer.from_json(json)
# print the JSON string representation of the object
print(NodeStatePeer.to_json())

# convert the object into a dict
node_state_peer_dict = node_state_peer_instance.to_dict()
# create an instance of NodeStatePeer from a dict
node_state_peer_from_dict = NodeStatePeer.from_dict(node_state_peer_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


