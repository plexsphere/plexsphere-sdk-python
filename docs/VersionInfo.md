# VersionInfo

Build metadata baked into the running binary.

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**version** | **str** | Semver tag of the release, e.g. \&quot;v0.1.0\&quot;. | 
**commit** | **str** | Git commit SHA the binary was built from. | 
**var_date** | **datetime** | RFC 3339 timestamp of the build. | 

## Example

```python
from plexsphere.models.version_info import VersionInfo

# TODO update the JSON string below
json = "{}"
# create an instance of VersionInfo from a JSON string
version_info_instance = VersionInfo.from_json(json)
# print the JSON string representation of the object
print(VersionInfo.to_json())

# convert the object into a dict
version_info_dict = version_info_instance.to_dict()
# create an instance of VersionInfo from a dict
version_info_from_dict = VersionInfo.from_dict(version_info_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


