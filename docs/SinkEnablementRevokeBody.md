# SinkEnablementRevokeBody

Body for `POST /v1/sink-enablements/{id}/revoke`. The `reason` is recorded on the decision and on the lifecycle outbox event, so the withdrawal carries an operator-supplied audit string. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**reason** | **str** | Revocation rationale. Non-empty — whitespace-only is rejected with &#x60;400 invalid_decision_reason&#x60;.  | 

## Example

```python
from plexsphere.models.sink_enablement_revoke_body import SinkEnablementRevokeBody

# TODO update the JSON string below
json = "{}"
# create an instance of SinkEnablementRevokeBody from a JSON string
sink_enablement_revoke_body_instance = SinkEnablementRevokeBody.from_json(json)
# print the JSON string representation of the object
print(SinkEnablementRevokeBody.to_json())

# convert the object into a dict
sink_enablement_revoke_body_dict = sink_enablement_revoke_body_instance.to_dict()
# create an instance of SinkEnablementRevokeBody from a dict
sink_enablement_revoke_body_from_dict = SinkEnablementRevokeBody.from_dict(sink_enablement_revoke_body_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


