# CloudAssignmentList

Page of Cloud Assignments returned by `GET /v1/projects/{id}/cloud-assignments`. The window is computed by the persistence layer in creation order; every row belongs to the one path Project. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**items** | [**List[CloudAssignmentResponse]**](CloudAssignmentResponse.md) | Cloud Assignments in the current page. | 
**next_cursor** | **str** | Continuation token for the next page. &#x60;null&#x60; or omitted when the iteration has reached end-of-stream. The encoding is HMAC-signed by the server so a tampered cursor surfaces as &#x60;400&#x60; on the next call.  | [optional] 

## Example

```python
from plexsphere.models.cloud_assignment_list import CloudAssignmentList

# TODO update the JSON string below
json = "{}"
# create an instance of CloudAssignmentList from a JSON string
cloud_assignment_list_instance = CloudAssignmentList.from_json(json)
# print the JSON string representation of the object
print(CloudAssignmentList.to_json())

# convert the object into a dict
cloud_assignment_list_dict = cloud_assignment_list_instance.to_dict()
# create an instance of CloudAssignmentList from a dict
cloud_assignment_list_from_dict = CloudAssignmentList.from_dict(cloud_assignment_list_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


