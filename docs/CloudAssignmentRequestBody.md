# CloudAssignmentRequestBody

Body for `POST /v1/projects/{id}/cloud-assignments`. Names the Cloud the Project requests usage of. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**cloud_id** | **UUID** | Identifier of the Cloud to request. Must be a non-zero UUID — a malformed value is rejected with &#x60;400 invalid_cloud_id&#x60;.  | 

## Example

```python
from plexsphere.models.cloud_assignment_request_body import CloudAssignmentRequestBody

# TODO update the JSON string below
json = "{}"
# create an instance of CloudAssignmentRequestBody from a JSON string
cloud_assignment_request_body_instance = CloudAssignmentRequestBody.from_json(json)
# print the JSON string representation of the object
print(CloudAssignmentRequestBody.to_json())

# convert the object into a dict
cloud_assignment_request_body_dict = cloud_assignment_request_body_instance.to_dict()
# create an instance of CloudAssignmentRequestBody from a dict
cloud_assignment_request_body_from_dict = CloudAssignmentRequestBody.from_dict(cloud_assignment_request_body_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


