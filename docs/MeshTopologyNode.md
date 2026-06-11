# MeshTopologyNode

Minimal Node projection embedded in the `nodes` array of `MeshTopology`. Carries only the fields the mesh-map view needs — a richer Node view lives on the per-Node read surfaces (`GET /v1/nodes/{id}/...`). 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**node_id** | **UUID** | Node identifier (UUIDv7). Joins back to the per-Node read surfaces for drill-down.  | 
**mesh_ip** | **str** | The Node&#39;s allocator-assigned mesh address — IPv4 or IPv6 depending on the Domain&#39;s mesh CIDR family.  | 
**reachability** | **str** | Per-Node reachability state at &#x60;generated_at&#x60;. Mirrors the enum values of the &#x60;Reachability&#x60; schema so a single renderer can colour Node markers and edge lines from one mapping.  | 

## Example

```python
from plexsphere.models.mesh_topology_node import MeshTopologyNode

# TODO update the JSON string below
json = "{}"
# create an instance of MeshTopologyNode from a JSON string
mesh_topology_node_instance = MeshTopologyNode.from_json(json)
# print the JSON string representation of the object
print(MeshTopologyNode.to_json())

# convert the object into a dict
mesh_topology_node_dict = mesh_topology_node_instance.to_dict()
# create an instance of MeshTopologyNode from a dict
mesh_topology_node_from_dict = MeshTopologyNode.from_dict(mesh_topology_node_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


