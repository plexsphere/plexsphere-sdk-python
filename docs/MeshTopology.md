# MeshTopology

Envelope returned by `GET /v1/domains/{domainId}/mesh/topology`. Carries a snapshot of every anchored Node in the Domain and the directed edges between them, captured at `generated_at`. The snapshot is a coherent read of the SSE peer-delta projection — partial pages are never emitted; a renderer can replace the entire view on each pull. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**domain_id** | **UUID** | The Domain the topology describes. Echoes the path parameter so a renderer can sanity-check the response against the request URL.  | 
**generated_at** | **datetime** | Server-side timestamp at which the snapshot was assembled. Renderers display it alongside the topology so an operator can spot a stale view.  | 
**nodes** | [**List[MeshTopologyNode]**](MeshTopologyNode.md) | Every anchored Peer Node in the Domain at &#x60;generated_at&#x60;. Order is stable but unspecified — renderers MUST NOT rely on the listing order for layout.  | 
**edges** | [**List[MeshEdge]**](MeshEdge.md) | Every directed mesh edge between two Nodes in &#x60;nodes&#x60; at &#x60;generated_at&#x60;. Edges are directional; a fully meshed pair of Nodes produces two entries.  | 

## Example

```python
from plexsphere.models.mesh_topology import MeshTopology

# TODO update the JSON string below
json = "{}"
# create an instance of MeshTopology from a JSON string
mesh_topology_instance = MeshTopology.from_json(json)
# print the JSON string representation of the object
print(MeshTopology.to_json())

# convert the object into a dict
mesh_topology_dict = mesh_topology_instance.to_dict()
# create an instance of MeshTopology from a dict
mesh_topology_from_dict = MeshTopology.from_dict(mesh_topology_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


