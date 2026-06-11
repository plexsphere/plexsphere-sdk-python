# IntegrityViolationRow

One integrity-violation row returned by `ListIntegrityViolations` — a metadata-only projection scoped to the owning Domain. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**id** | **UUID** | Integrity-violation identifier (UUIDv7). | 
**node_id** | **UUID** | Node that reported the violation (UUIDv7). | 
**domain_id** | **UUID** | Owning Domain (UUIDv7) — the residency pivot the per-row visibility filter authorises against.  | 
**kind** | [**IntegrityViolationKind**](IntegrityViolationKind.md) |  | 
**status** | [**IntegrityViolationStatus**](IntegrityViolationStatus.md) |  | 
**artifact_id** | **str** | Stable identifier of the affected artifact (hook name, binary path label, or host-key file label).  | 
**detected_at** | **datetime** | Timestamp the agent detected the violation (UTC). | 
**acknowledged_at** | **datetime** | Timestamp an operator acknowledged the violation (UTC). &#x60;null&#x60; or omitted while the violation is still &#x60;open&#x60;.  | [optional] 
**acknowledged_by_subject** | **str** | Subject string of the operator that acknowledged the violation. &#x60;null&#x60; or omitted while the violation is still &#x60;open&#x60;.  | [optional] 
**acknowledge_reason** | **str** | Free-text rationale recorded with the acknowledgement. &#x60;null&#x60; or omitted while the violation is still &#x60;open&#x60;.  | [optional] 

## Example

```python
from plexsphere.models.integrity_violation_row import IntegrityViolationRow

# TODO update the JSON string below
json = "{}"
# create an instance of IntegrityViolationRow from a JSON string
integrity_violation_row_instance = IntegrityViolationRow.from_json(json)
# print the JSON string representation of the object
print(IntegrityViolationRow.to_json())

# convert the object into a dict
integrity_violation_row_dict = integrity_violation_row_instance.to_dict()
# create an instance of IntegrityViolationRow from a dict
integrity_violation_row_from_dict = IntegrityViolationRow.from_dict(integrity_violation_row_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


