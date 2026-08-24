# SinkEnablement

One Project's permission to deliver telemetry to one sink of that Project's own Domain. It names the Project, the sink, the lifecycle state, the principal that asked for the grant, and the decision that settled it. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**id** | **UUID** | Stable identifier of the enablement (UUIDv7). | 
**project_id** | **UUID** | Project the grant is filed for. | 
**sink_id** | **UUID** | Sink the grant addresses. | 
**state** | [**SinkEnablementState**](SinkEnablementState.md) |  | 
**requested_by** | **UUID** | Principal that asked for the grant. | 
**decided_by_subject** | **str** | ReBAC subject of the principal that moved the enablement out of &#x60;requested&#x60;. Absent while the enablement is undecided.  | [optional] 
**decided_at** | **datetime** | RFC 3339 timestamp the decision was recorded. Absent while the enablement is undecided.  | [optional] 
**decision_reason** | **str** | Rationale recorded with the decision. A rejection and a revocation both carry one; an approval records none, so the field is absent on an approved enablement.  | [optional] 
**created_at** | **datetime** | RFC 3339 timestamp the enablement was filed. | 
**sync_pending** | **bool** | Set on a grant response only. &#x60;true&#x60; means the grant COMMITTED — the row moved and its event is durable — but the arm that mirrors it into the authorization graph has not completed, so the grant is not yet effective. The caller must NOT retry: the retry is refused by the live-unique index and reports a conflict for work that already happened. The committed event drives the same mutation through the authz-sync arm.  | [optional] 

## Example

```python
from plexsphere.models.sink_enablement import SinkEnablement

# TODO update the JSON string below
json = "{}"
# create an instance of SinkEnablement from a JSON string
sink_enablement_instance = SinkEnablement.from_json(json)
# print the JSON string representation of the object
print(SinkEnablement.to_json())

# convert the object into a dict
sink_enablement_dict = sink_enablement_instance.to_dict()
# create an instance of SinkEnablement from a dict
sink_enablement_from_dict = SinkEnablement.from_dict(sink_enablement_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


