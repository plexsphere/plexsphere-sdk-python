# RotationImpactEdge

Single peer edge whose pairwise PSK would be re-issued by a mesh-key rotation against the previewed Node. Carries the peer Node identifier together with the `(kid, wrap_key_version)` reference of the PSK currently in force — the same reference the mutating `POST /v1/keys/rotate` would replace. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**peer_node_id** | **UUID** | Peer at the other end of the affected edge — joins back to the per-Node read surfaces for drill-down.  | 
**current_kid** | **str** | Key id of the PSK currently in force on this edge. Mirrors the &#x60;kid&#x60; field on &#x60;KeysRotateResponse&#x60; so a renderer can display \&quot;before / after\&quot; the dry-run with a single mapping.  | 
**current_wrap_key_version** | **int** | Version of the active wrap key under which the in-force PSK is wrapped. Paired with &#x60;current_kid&#x60; it pins the exact wrapping epoch the rotation would supersede.  | 

## Example

```python
from plexsphere.models.rotation_impact_edge import RotationImpactEdge

# TODO update the JSON string below
json = "{}"
# create an instance of RotationImpactEdge from a JSON string
rotation_impact_edge_instance = RotationImpactEdge.from_json(json)
# print the JSON string representation of the object
print(RotationImpactEdge.to_json())

# convert the object into a dict
rotation_impact_edge_dict = rotation_impact_edge_instance.to_dict()
# create an instance of RotationImpactEdge from a dict
rotation_impact_edge_from_dict = RotationImpactEdge.from_dict(rotation_impact_edge_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


