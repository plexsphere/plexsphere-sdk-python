# CloudAssignmentDecisionRequest

Body for `POST /v1/cloud-assignments/{id}/reject` and `POST /v1/cloud-assignments/{id}/revoke`. The `reason` is recorded on the lifecycle outbox event so the decision carries an operator-supplied audit string. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**reason** | **str** | Decision rationale. Non-empty — whitespace-only is rejected with &#x60;400 invalid_decision_reason&#x60;.  | 

## Example

```python
from plexsphere.models.cloud_assignment_decision_request import CloudAssignmentDecisionRequest

# TODO update the JSON string below
json = "{}"
# create an instance of CloudAssignmentDecisionRequest from a JSON string
cloud_assignment_decision_request_instance = CloudAssignmentDecisionRequest.from_json(json)
# print the JSON string representation of the object
print(CloudAssignmentDecisionRequest.to_json())

# convert the object into a dict
cloud_assignment_decision_request_dict = cloud_assignment_decision_request_instance.to_dict()
# create an instance of CloudAssignmentDecisionRequest from a dict
cloud_assignment_decision_request_from_dict = CloudAssignmentDecisionRequest.from_dict(cloud_assignment_decision_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


