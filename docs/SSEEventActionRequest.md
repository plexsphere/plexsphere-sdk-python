# SSEEventActionRequest

Payload of the `action_request` Signed Event Bus event delivered to the target Node's event stream. The Node runs the dispatched action from this envelope and reports its terminal result back to the carried callback URL. One Execution fans out to one event per target Node, so two targets of the same dispatch share an `execution_id` but carry distinct `node_id` and `event_id`. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**event_id** | **UUID** | Fresh per-dispatch-event identifier (UUIDv7), distinct from &#x60;execution_id&#x60; so the event carries its own identity.  | 
**occurred_at** | **datetime** | Timestamp the event was produced (UTC). | 
**execution_id** | **UUID** | Identifier of the Execution the dispatch belongs to (UUIDv7). | 
**node_id** | **UUID** | The single target Node this event addresses (UUIDv7). | 
**action** | **str** | Name of the dispatched action. | 
**type** | [**ActionKind**](ActionKind.md) |  | 
**parameters** | **Dict[str, object]** | Opaque JSON parameter document passed verbatim to the action. &#x60;null&#x60; when the dispatch carries no parameters.  | [optional] 
**timeout_seconds** | **int** | Per-dispatch timeout in whole seconds the Node must report a terminal result within.  | 
**callback_url** | **str** | Absolute URL the Node reports its result back to, built per the template &#x60;{base}/v1/nodes/{node_id}/executions/{execution_id}&#x60;.  | 

## Example

```python
from plexsphere.models.sse_event_action_request import SSEEventActionRequest

# TODO update the JSON string below
json = "{}"
# create an instance of SSEEventActionRequest from a JSON string
sse_event_action_request_instance = SSEEventActionRequest.from_json(json)
# print the JSON string representation of the object
print(SSEEventActionRequest.to_json())

# convert the object into a dict
sse_event_action_request_dict = sse_event_action_request_instance.to_dict()
# create an instance of SSEEventActionRequest from a dict
sse_event_action_request_from_dict = SSEEventActionRequest.from_dict(sse_event_action_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


