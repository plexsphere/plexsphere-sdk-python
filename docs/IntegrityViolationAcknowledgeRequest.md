# IntegrityViolationAcknowledgeRequest

Body for `POST /v1/integrity-violations/{id}/acknowledge`. The `reason` is recorded on the triage row as the operator-supplied rationale for moving the violation out of the `open` state. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**reason** | **str** | Acknowledgement rationale. Non-empty — whitespace-only is rejected with &#x60;400 invalid_acknowledge_reason&#x60;.  | 

## Example

```python
from plexsphere.models.integrity_violation_acknowledge_request import IntegrityViolationAcknowledgeRequest

# TODO update the JSON string below
json = "{}"
# create an instance of IntegrityViolationAcknowledgeRequest from a JSON string
integrity_violation_acknowledge_request_instance = IntegrityViolationAcknowledgeRequest.from_json(json)
# print the JSON string representation of the object
print(IntegrityViolationAcknowledgeRequest.to_json())

# convert the object into a dict
integrity_violation_acknowledge_request_dict = integrity_violation_acknowledge_request_instance.to_dict()
# create an instance of IntegrityViolationAcknowledgeRequest from a dict
integrity_violation_acknowledge_request_from_dict = IntegrityViolationAcknowledgeRequest.from_dict(integrity_violation_acknowledge_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


