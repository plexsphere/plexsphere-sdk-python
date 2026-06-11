# PlexdArtifactRef

One indexed plexd release line returned by `ListPlexdArtifacts` — a list-row projection carrying the release version, the lifecycle support status, the supply-chain verdict, the per-architecture checksums, and the index timestamps. The verbatim Sigstore bundle is served separately by the per-version `.sigstore` endpoint rather than inlined here. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**version** | **str** | Upstream plexd release tag the registry indexes. | 
**support_status** | **str** | Lifecycle status of the release line. &#x60;supported&#x60; is a fully supported, eligible upgrade target; &#x60;deprecated&#x60; still verifies but operators should migrate off it; &#x60;withdrawn&#x60; is no longer a valid upgrade target.  | 
**verdict** | **str** | Supply-chain verification verdict. &#x60;verified&#x60; means every artifact passed the Sigstore bundle and checksum gate; &#x60;unverified&#x60; marks a record that has not passed the gate.  | 
**architectures** | [**List[PlexdArtifactArch]**](PlexdArtifactArch.md) | The per-architecture builds of this release with their recorded SHA-256 checksums.  | 
**created_at** | **datetime** | Timestamp the registry first indexed this release line (UTC). Absent when the registry does not record it.  | [optional] 
**updated_at** | **datetime** | Timestamp the registry last refreshed this release line (UTC). Absent when the registry does not record it.  | [optional] 

## Example

```python
from plexsphere.models.plexd_artifact_ref import PlexdArtifactRef

# TODO update the JSON string below
json = "{}"
# create an instance of PlexdArtifactRef from a JSON string
plexd_artifact_ref_instance = PlexdArtifactRef.from_json(json)
# print the JSON string representation of the object
print(PlexdArtifactRef.to_json())

# convert the object into a dict
plexd_artifact_ref_dict = plexd_artifact_ref_instance.to_dict()
# create an instance of PlexdArtifactRef from a dict
plexd_artifact_ref_from_dict = PlexdArtifactRef.from_dict(plexd_artifact_ref_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


