# CloudAssignmentGrantRequest

Body for `POST /v1/clouds/{id}/cloud-assignments`. Names the Project the operator grants the Cloud to. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**project_id** | **UUID** | Identifier of the Project to grant. Must be a non-zero UUID — a malformed value is rejected with &#x60;400 invalid_project_id&#x60;.  | 

## Example

```python
from plexsphere.models.cloud_assignment_grant_request import CloudAssignmentGrantRequest

# TODO update the JSON string below
json = "{}"
# create an instance of CloudAssignmentGrantRequest from a JSON string
cloud_assignment_grant_request_instance = CloudAssignmentGrantRequest.from_json(json)
# print the JSON string representation of the object
print(CloudAssignmentGrantRequest.to_json())

# convert the object into a dict
cloud_assignment_grant_request_dict = cloud_assignment_grant_request_instance.to_dict()
# create an instance of CloudAssignmentGrantRequest from a dict
cloud_assignment_grant_request_from_dict = CloudAssignmentGrantRequest.from_dict(cloud_assignment_grant_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


