# ResourceList

Page of Resources returned by `GET /v1/projects/{project_id}/resources`. The window is computed by the persistence layer in creation order; per-row visibility is layered on top so the `items` array is the subset the caller is authorised to see. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**items** | [**List[ResourceResponse]**](ResourceResponse.md) | Resources in the current page. | 
**next_cursor** | **str** | Continuation token for the next page. &#x60;null&#x60; or omitted when the iteration has reached end-of-stream. The encoding is HMAC-signed by the server so a tampered cursor surfaces as &#x60;400&#x60; on the next call.  | [optional] 

## Example

```python
from plexsphere.models.resource_list import ResourceList

# TODO update the JSON string below
json = "{}"
# create an instance of ResourceList from a JSON string
resource_list_instance = ResourceList.from_json(json)
# print the JSON string representation of the object
print(ResourceList.to_json())

# convert the object into a dict
resource_list_dict = resource_list_instance.to_dict()
# create an instance of ResourceList from a dict
resource_list_from_dict = ResourceList.from_dict(resource_list_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


