# RegisterPeer

Minimal projection of an existing Domain member that the freshly-registered Node needs in order to bring its WireGuard table up. The shape is deliberately narrow: anything else (labels, resource handle, audit metadata) belongs on later state-pull responses, not on the bootstrap payload. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**node_id** | **UUID** | Peer&#39;s Node identifier. Plexd&#39;s reconcile loop keys peers by id.  | 
**mesh_ip** | **str** | Peer&#39;s allocator-assigned mesh address. Plexd programs the wireguard endpoint from this field.  | 
**public_key** | **str** | Peer&#39;s WireGuard public key, base64-encoded with standard padding.  | 
**fallback_endpoint** | **str** | Canonical bridge &#x60;host:port&#x60; that plexd programs as a NAT-traversal relay when direct peer-to-peer connectivity is unavailable. Empty or absent means no bridge is currently assigned for this peer. Format: &#x60;&lt;inet-or-hostname&gt;:&lt;port&gt;&#x60;.  | [optional] 

## Example

```python
from plexsphere.models.register_peer import RegisterPeer

# TODO update the JSON string below
json = "{}"
# create an instance of RegisterPeer from a JSON string
register_peer_instance = RegisterPeer.from_json(json)
# print the JSON string representation of the object
print(RegisterPeer.to_json())

# convert the object into a dict
register_peer_dict = register_peer_instance.to_dict()
# create an instance of RegisterPeer from a dict
register_peer_from_dict = RegisterPeer.from_dict(register_peer_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


