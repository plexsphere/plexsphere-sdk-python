# NodeStateExecution

A single pending action dispatch projected onto the `NodeStateSnapshot` envelope. It mirrors the `action_request` Signed Event Bus payload minus the event-envelope fields (`event_id`, `occurred_at`, `node_id`) and the callback URL: the agent derives its callback path from its configured base per the agent contract. The pull carries the absolute `expires_at` deadline rather than the event payload's relative timeout because pull delivery is delayed by the reconcile cadence. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**execution_id** | **UUID** | Identifier of the Execution the dispatch belongs to (UUIDv7). | 
**action** | **str** | Name of the dispatched action. | 
**type** | [**ActionKind**](ActionKind.md) |  | 
**parameters** | **Dict[str, object]** | Opaque JSON parameter document passed verbatim to the action. &#x60;null&#x60; when the dispatch carries no parameters.  | [optional] 
**status** | **str** | Per-target status of the dispatch at snapshot time so a re-pulling agent knows what it has already acknowledged. One of &#x60;pending&#x60;, &#x60;ack&#x60;, or &#x60;started&#x60;; an entry leaves the block once its target reaches a terminal status.  | 
**requested_at** | **datetime** | Timestamp the dispatch was requested (UTC). | 
**expires_at** | **datetime** | Absolute deadline (UTC) the Node must report a terminal result within. The pull carries the absolute expiry rather than the event payload&#39;s relative &#x60;timeout_seconds&#x60; because pull delivery is delayed by the reconcile cadence.  | 

## Example

```python
from plexsphere.models.node_state_execution import NodeStateExecution

# TODO update the JSON string below
json = "{}"
# create an instance of NodeStateExecution from a JSON string
node_state_execution_instance = NodeStateExecution.from_json(json)
# print the JSON string representation of the object
print(NodeStateExecution.to_json())

# convert the object into a dict
node_state_execution_dict = node_state_execution_instance.to_dict()
# create an instance of NodeStateExecution from a dict
node_state_execution_from_dict = NodeStateExecution.from_dict(node_state_execution_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


