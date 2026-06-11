# CredentialAssignmentResponse

Metadata projection of a Credential Assignment. The shape is shared by `RequestCredentialAssignment`, `ListCredentialAssignments`, `ApproveCredentialAssignment`, `RejectCredentialAssignment`, and `RevokeCredentialAssignment` so clients only need one binding. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**id** | **UUID** | Credential Assignment identifier (UUIDv7). | 
**project_id** | **UUID** | Identifier of the owning Project — the residency pivot the ReBAC gate authorises against.  | 
**cloud_credential_id** | **UUID** | Identifier of the Cloud Credential bound to the Project by this assignment.  | 
**state** | [**CredentialAssignmentState**](CredentialAssignmentState.md) |  | 
**materialised** | **bool** | Whether the assignment&#39;s binding is currently live. &#x60;true&#x60; only while the assignment is in the &#x60;approved&#x60; state; &#x60;false&#x60; for &#x60;requested&#x60;, &#x60;rejected&#x60;, and &#x60;revoked&#x60;.  | 
**created_at** | **datetime** | Aggregate creation timestamp (UTC). | 
**updated_at** | **datetime** | Last-modified timestamp (UTC). Bumped by every lifecycle transition — approve, reject, revoke.  | 

## Example

```python
from plexsphere.models.credential_assignment_response import CredentialAssignmentResponse

# TODO update the JSON string below
json = "{}"
# create an instance of CredentialAssignmentResponse from a JSON string
credential_assignment_response_instance = CredentialAssignmentResponse.from_json(json)
# print the JSON string representation of the object
print(CredentialAssignmentResponse.to_json())

# convert the object into a dict
credential_assignment_response_dict = credential_assignment_response_instance.to_dict()
# create an instance of CredentialAssignmentResponse from a dict
credential_assignment_response_from_dict = CredentialAssignmentResponse.from_dict(credential_assignment_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


