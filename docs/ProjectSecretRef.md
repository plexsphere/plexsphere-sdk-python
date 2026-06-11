# ProjectSecretRef

One entry of a Project's Secret Store inventory — the secret `name` and its `current_version`. The projection is metadata-only and NEVER carries a secret payload, an OpenBao path, or any wrapping material. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**name** | **str** | Secret name within the owning Project. Lower-case, starts with a letter, and is limited to letters, digits, hyphen, and underscore (1 to 63 characters).  | 
**current_version** | **int** | The currently-served KV v2 version of the secret — a small, monotonically-increasing integer that starts at 1.  | 

## Example

```python
from plexsphere.models.project_secret_ref import ProjectSecretRef

# TODO update the JSON string below
json = "{}"
# create an instance of ProjectSecretRef from a JSON string
project_secret_ref_instance = ProjectSecretRef.from_json(json)
# print the JSON string representation of the object
print(ProjectSecretRef.to_json())

# convert the object into a dict
project_secret_ref_dict = project_secret_ref_instance.to_dict()
# create an instance of ProjectSecretRef from a dict
project_secret_ref_from_dict = ProjectSecretRef.from_dict(project_secret_ref_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


