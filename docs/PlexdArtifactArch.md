# PlexdArtifactArch

One per-architecture build of a plexd release — its target architecture and the recorded SHA-256 checksum of the build's plexd binary. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**architecture** | **str** | Target CPU architecture of the build (for example &#x60;amd64&#x60; or &#x60;arm64&#x60;). The set is owned by the upstream release pipeline, not by plexsphere, so it is not constrained to a closed enum.  | 
**checksum** | **str** | Recorded SHA-256 checksum of the build&#39;s plexd binary — the raw 32-byte digest, hex-encoded as 64 lowercase hex characters.  | 

## Example

```python
from plexsphere.models.plexd_artifact_arch import PlexdArtifactArch

# TODO update the JSON string below
json = "{}"
# create an instance of PlexdArtifactArch from a JSON string
plexd_artifact_arch_instance = PlexdArtifactArch.from_json(json)
# print the JSON string representation of the object
print(PlexdArtifactArch.to_json())

# convert the object into a dict
plexd_artifact_arch_dict = plexd_artifact_arch_instance.to_dict()
# create an instance of PlexdArtifactArch from a dict
plexd_artifact_arch_from_dict = PlexdArtifactArch.from_dict(plexd_artifact_arch_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


