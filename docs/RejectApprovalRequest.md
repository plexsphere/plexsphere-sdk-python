# RejectApprovalRequest

Body for `POST /v1/approvals/{id}/reject`. The `reason` is recorded on the decision so the rejection carries an operator-supplied audit string. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**reason** | **str** | Decision rationale. Non-empty — whitespace-only is rejected with &#x60;400 invalid_decision_reason&#x60;.  | 

## Example

```python
from plexsphere.models.reject_approval_request import RejectApprovalRequest

# TODO update the JSON string below
json = "{}"
# create an instance of RejectApprovalRequest from a JSON string
reject_approval_request_instance = RejectApprovalRequest.from_json(json)
# print the JSON string representation of the object
print(RejectApprovalRequest.to_json())

# convert the object into a dict
reject_approval_request_dict = reject_approval_request_instance.to_dict()
# create an instance of RejectApprovalRequest from a dict
reject_approval_request_from_dict = RejectApprovalRequest.from_dict(reject_approval_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


