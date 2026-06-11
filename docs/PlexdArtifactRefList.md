# PlexdArtifactRefList

Page of indexed plexd releases returned by `ListPlexdArtifacts`. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**items** | [**List[PlexdArtifactRef]**](PlexdArtifactRef.md) | Indexed plexd releases in the current page. | 
**next_cursor** | **str** | Continuation token for the next page. &#x60;null&#x60; or omitted when the iteration has reached end-of-stream. The encoding is HMAC-signed so a tampered cursor surfaces as &#x60;400&#x60; on the next call.  | [optional] 

## Example

```python
from plexsphere.models.plexd_artifact_ref_list import PlexdArtifactRefList

# TODO update the JSON string below
json = "{}"
# create an instance of PlexdArtifactRefList from a JSON string
plexd_artifact_ref_list_instance = PlexdArtifactRefList.from_json(json)
# print the JSON string representation of the object
print(PlexdArtifactRefList.to_json())

# convert the object into a dict
plexd_artifact_ref_list_dict = plexd_artifact_ref_list_instance.to_dict()
# create an instance of PlexdArtifactRefList from a dict
plexd_artifact_ref_list_from_dict = PlexdArtifactRefList.from_dict(plexd_artifact_ref_list_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


