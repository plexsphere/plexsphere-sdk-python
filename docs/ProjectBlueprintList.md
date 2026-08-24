# ProjectBlueprintList

Page of Blueprint offers returned by `GET /v1/projects/{id}/blueprints`. The window is computed by the persistence layer in slug order; per-row visibility is layered on top so the `items` array is the subset the caller is authorised to see. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**items** | [**List[ProjectBlueprintOffer]**](ProjectBlueprintOffer.md) | Blueprint offers in the current page. | 
**reachable_provider_kinds** | **List[str]** | The substrates the Project reaches through its &#x60;approved&#x60; Cloud Assignments, deduplicated and sorted ascending. An empty array means the Project holds no approved assignment whose Cloud provider the correspondence knows, so every item is &#x60;provisionable: false&#x60;.  | 
**next_cursor** | **str** | Continuation token for the next page. Absent when the iteration has reached end-of-stream. The encoding is HMAC-signed by the server so a tampered cursor surfaces as &#x60;400&#x60; on the next call.  | [optional] 

## Example

```python
from plexsphere.models.project_blueprint_list import ProjectBlueprintList

# TODO update the JSON string below
json = "{}"
# create an instance of ProjectBlueprintList from a JSON string
project_blueprint_list_instance = ProjectBlueprintList.from_json(json)
# print the JSON string representation of the object
print(ProjectBlueprintList.to_json())

# convert the object into a dict
project_blueprint_list_dict = project_blueprint_list_instance.to_dict()
# create an instance of ProjectBlueprintList from a dict
project_blueprint_list_from_dict = ProjectBlueprintList.from_dict(project_blueprint_list_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


