# CredentialAssignmentDecisionRequest

Body for `POST /v1/credential-assignments/{id}/reject` and `POST /v1/credential-assignments/{id}/revoke`. The `reason` is recorded on the lifecycle outbox event so the decision carries an approver- or operator-supplied audit string. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**reason** | **str** | Decision rationale. Non-empty — whitespace-only is rejected with &#x60;400 invalid_decision_reason&#x60;.  | 

## Example

```python
from plexsphere.models.credential_assignment_decision_request import CredentialAssignmentDecisionRequest

# TODO update the JSON string below
json = "{}"
# create an instance of CredentialAssignmentDecisionRequest from a JSON string
credential_assignment_decision_request_instance = CredentialAssignmentDecisionRequest.from_json(json)
# print the JSON string representation of the object
print(CredentialAssignmentDecisionRequest.to_json())

# convert the object into a dict
credential_assignment_decision_request_dict = credential_assignment_decision_request_instance.to_dict()
# create an instance of CredentialAssignmentDecisionRequest from a dict
credential_assignment_decision_request_from_dict = CredentialAssignmentDecisionRequest.from_dict(credential_assignment_decision_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


