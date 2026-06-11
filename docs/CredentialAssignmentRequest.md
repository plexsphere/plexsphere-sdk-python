# CredentialAssignmentRequest

Body for `POST /v1/projects/{id}/credential-assignments`. Names the Cloud Credential to bind to the Project. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**cloud_credential_id** | **UUID** | Identifier of the Cloud Credential to bind. Must be a non-zero UUID — a malformed value is rejected with &#x60;400 invalid_cloud_credential_id&#x60;.  | 

## Example

```python
from plexsphere.models.credential_assignment_request import CredentialAssignmentRequest

# TODO update the JSON string below
json = "{}"
# create an instance of CredentialAssignmentRequest from a JSON string
credential_assignment_request_instance = CredentialAssignmentRequest.from_json(json)
# print the JSON string representation of the object
print(CredentialAssignmentRequest.to_json())

# convert the object into a dict
credential_assignment_request_dict = credential_assignment_request_instance.to_dict()
# create an instance of CredentialAssignmentRequest from a dict
credential_assignment_request_from_dict = CredentialAssignmentRequest.from_dict(credential_assignment_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


