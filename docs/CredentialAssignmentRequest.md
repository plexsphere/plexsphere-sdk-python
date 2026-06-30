# CredentialAssignmentRequest

Body for `POST /v1/projects/{id}/credential-assignments`. Names the Cloud Credential to bind to the Project — either directly by `cloud_credential_id`, or indirectly by `cloud_id`, in which case the system auto-selects the most recently issued eligible credential serving that Cloud.  Exactly one of `cloud_credential_id` or `cloud_id` MUST be supplied. Supplying both is rejected with `400 ambiguous_credential_target`; supplying neither is rejected with `400 invalid_body`. The `cloud_id` form additionally requires the Cloud to be usable in the Project (an approved Cloud Assignment) — otherwise `422 cloud_not_usable_in_project` — and requires at least one eligible credential serving the Cloud — otherwise `422 no_eligible_credential_for_cloud`. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**cloud_credential_id** | **UUID** | Identifier of the Cloud Credential to bind directly. Must be a non-zero UUID — a malformed value is rejected with &#x60;400 invalid_cloud_credential_id&#x60;. Mutually exclusive with &#x60;cloud_id&#x60;.  | [optional] 
**cloud_id** | **UUID** | Identifier of the Cloud whose newest eligible credential the system auto-selects and binds. Must be a non-zero UUID — a malformed value is rejected with &#x60;400 invalid_cloud_id&#x60;. Mutually exclusive with &#x60;cloud_credential_id&#x60;.  | [optional] 

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


