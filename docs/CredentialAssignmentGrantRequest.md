# CredentialAssignmentGrantRequest

Body for `POST /v1/cloud-credentials/{id}/credential-assignments`. Names the Project the credential's holder grants it to. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**project_id** | **UUID** | Identifier of the Project to grant. Must be a non-zero UUID — a malformed value is rejected with &#x60;400 invalid_project_id&#x60;.  | 

## Example

```python
from plexsphere.models.credential_assignment_grant_request import CredentialAssignmentGrantRequest

# TODO update the JSON string below
json = "{}"
# create an instance of CredentialAssignmentGrantRequest from a JSON string
credential_assignment_grant_request_instance = CredentialAssignmentGrantRequest.from_json(json)
# print the JSON string representation of the object
print(CredentialAssignmentGrantRequest.to_json())

# convert the object into a dict
credential_assignment_grant_request_dict = credential_assignment_grant_request_instance.to_dict()
# create an instance of CredentialAssignmentGrantRequest from a dict
credential_assignment_grant_request_from_dict = CredentialAssignmentGrantRequest.from_dict(credential_assignment_grant_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


