# PlexdArtifact

One indexed plexd release line returned by `GetPlexdArtifact`. It carries the release `version`, every per-architecture build, the lifecycle `support_status`, and the `verdict` the supply-chain verification gate pronounced. The verbatim Sigstore bundle attesting each build is served separately by the `.sigstore` companion endpoint rather than inlined here. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**version** | **str** | Upstream plexd release tag the registry indexes (for example a semver tag such as &#x60;v1.4.2&#x60;).  | 
**support_status** | **str** | Closed-set lifecycle status of the release line. &#x60;supported&#x60; is a fully supported, eligible upgrade target; &#x60;deprecated&#x60; still verifies and is served but operators should migrate off it; &#x60;withdrawn&#x60; is no longer a valid upgrade target. The wire values are intentionally lowercase and map onto the domain&#39;s support-status literals.  | 
**verdict** | **str** | Closed-set verification verdict the supply-chain gate pronounced on the release. &#x60;verified&#x60; means every artifact passed the Sigstore bundle and checksum verification gate; &#x60;unverified&#x60; marks a record that has not passed the gate and is never served as an eligible upgrade target.  | 
**architectures** | [**List[PlexdArtifactArch]**](PlexdArtifactArch.md) | The per-architecture builds of this release. At least one build is always present, and no two builds share an architecture.  | 

## Example

```python
from plexsphere.models.plexd_artifact import PlexdArtifact

# TODO update the JSON string below
json = "{}"
# create an instance of PlexdArtifact from a JSON string
plexd_artifact_instance = PlexdArtifact.from_json(json)
# print the JSON string representation of the object
print(PlexdArtifact.to_json())

# convert the object into a dict
plexd_artifact_dict = plexd_artifact_instance.to_dict()
# create an instance of PlexdArtifact from a dict
plexd_artifact_from_dict = PlexdArtifact.from_dict(plexd_artifact_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


