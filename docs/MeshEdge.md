# MeshEdge

Single directed edge in the per-Domain mesh-topology projection. Mirrors a SSE peer-delta row one-for-one — a renderer that consumes the live stream and a renderer that polls `GET /v1/domains/{domainId}/mesh/topology` converge on byte-identical state. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**source_node_id** | **UUID** | Node from which the WireGuard handshake originates. Pinned against the live Peer aggregate so a stale read never returns an edge for a retired Node.  | 
**target_node_id** | **UUID** | Peer Node the handshake terminates against. Same liveness guarantee as &#x60;source_node_id&#x60;.  | 
**mode** | **str** | &#x60;direct&#x60; when the edge is using a direct WireGuard handshake against the peer&#39;s NAT-observed endpoint; &#x60;relayed&#x60; when the bridge fallback endpoint assigned to this pair is currently in use.  | 
**relay_node_id** | **UUID** | Bridge Node currently serving as the relay fallback for this edge. Always populated when &#x60;mode&#x60; is &#x60;relayed&#x60;; always &#x60;null&#x60; when &#x60;mode&#x60; is &#x60;direct&#x60;.  | 
**status** | **str** | Derived edge health, computed by applying the per-Domain reachability policy thresholds to the per-edge handshake observation. The handshake telemetry itself is deliberately NOT exposed on this envelope — see the &#x60;MeshTopologyPage&#x60; handshake-telemetry follow-up anchor for the rationale and the planned schema extension. Uses the same enum values as the per-Node &#x60;Reachability&#x60; projection so a single renderer can colour both surfaces from one mapping.  | 

## Example

```python
from plexsphere.models.mesh_edge import MeshEdge

# TODO update the JSON string below
json = "{}"
# create an instance of MeshEdge from a JSON string
mesh_edge_instance = MeshEdge.from_json(json)
# print the JSON string representation of the object
print(MeshEdge.to_json())

# convert the object into a dict
mesh_edge_dict = mesh_edge_instance.to_dict()
# create an instance of MeshEdge from a dict
mesh_edge_from_dict = MeshEdge.from_dict(mesh_edge_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


