# RotationImpactPreview

Body for GET /v1/nodes/{id}/keys/rotate/preview success responses. Carries a non-mutating dry-run of the fleet impact a mesh-key rotation against the addressed Node would have: the live Peer the rotation would target, the listing of peer edges whose PSK would be re-issued, whether a rotation is already in flight, and an estimated wall-clock duration so the dashboard confirmation dialog can size its progress indicator.  The response carries NO PSK plaintext and NO ciphertext, and the handler appends NO `peer_key_rotation` row, NO `rotate_keys` outbox event, and NO audit row beyond the read- side ReBAC decision — calling this surface twice in a row is guaranteed to be a no-op against the persistence layer. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**node_id** | **UUID** | The Node whose rotation is being previewed. Echoes the path parameter so a renderer can sanity-check the response against the request URL.  | 
**peer_id** | **UUID** | Identifier of the live Peer the rotation would target. Mirrors &#x60;RotationTriggerResponse.peer_id&#x60; so the preview and the mutating trigger share a single mapping.  | 
**affected_peer_count** | **int** | Number of peers whose pairwise PSK would be re-issued by the rotation. Equal to the length of &#x60;affected_edges&#x60;; duplicated as a top-level scalar so a renderer can show the count without expanding the edge listing.  | 
**affected_edges** | [**List[RotationImpactEdge]**](RotationImpactEdge.md) | Explicit listing of peer edges that would receive a fresh pairwise PSK. Order is stable but unspecified — renderers MUST NOT rely on the listing order for layout.  | 
**already_pending** | **bool** | True when a &#x60;peer_key_rotation&#x60; row is already pending for the addressed Node — i.e. a live &#x60;POST /v1/nodes/{id}/keys/rotate&#x60; would surface the same flag and append no second event. Operators switch on this flag to distinguish a fresh trigger from an idempotent no-op before confirming the rotation.  | 
**estimated_duration_seconds** | **int** | Estimated wall-clock seconds the rotation will take, based on the current SSE fan-out and the affected peer count. Capacity planning hint — not a guarantee.  | 

## Example

```python
from plexsphere.models.rotation_impact_preview import RotationImpactPreview

# TODO update the JSON string below
json = "{}"
# create an instance of RotationImpactPreview from a JSON string
rotation_impact_preview_instance = RotationImpactPreview.from_json(json)
# print the JSON string representation of the object
print(RotationImpactPreview.to_json())

# convert the object into a dict
rotation_impact_preview_dict = rotation_impact_preview_instance.to_dict()
# create an instance of RotationImpactPreview from a dict
rotation_impact_preview_from_dict = RotationImpactPreview.from_dict(rotation_impact_preview_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


