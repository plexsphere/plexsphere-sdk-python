# ProjectSecretList

Page of Secret Store inventory entries returned by `GET /v1/projects/{project_id}/secrets`. The window is computed by the persistence layer in ascending name order. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**items** | [**List[ProjectSecretRef]**](ProjectSecretRef.md) | Secret inventory entries in the current page. | 
**next_cursor** | **str** | Continuation token for the next page. &#x60;null&#x60; or omitted when the iteration has reached end-of-stream. The encoding is HMAC-signed by the server so a tampered cursor surfaces as &#x60;400&#x60; on the next call.  | [optional] 

## Example

```python
from plexsphere.models.project_secret_list import ProjectSecretList

# TODO update the JSON string below
json = "{}"
# create an instance of ProjectSecretList from a JSON string
project_secret_list_instance = ProjectSecretList.from_json(json)
# print the JSON string representation of the object
print(ProjectSecretList.to_json())

# convert the object into a dict
project_secret_list_dict = project_secret_list_instance.to_dict()
# create an instance of ProjectSecretList from a dict
project_secret_list_from_dict = ProjectSecretList.from_dict(project_secret_list_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


