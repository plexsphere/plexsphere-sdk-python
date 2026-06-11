# SSEEventSessionSetup

Payload of the `session_setup` Signed Event Bus event delivered to the target Node's event stream. The Node opens its on-node listener for the session from this envelope. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**session_id** | **UUID** | Identifier of the Session being set up (UUIDv7). | 
**jti** | **str** | The issued token&#39;s &#x60;jti&#x60;. Equals the session identifier.  | 
**kind** | [**SessionKind**](SessionKind.md) |  | 
**target** | [**SessionTarget**](SessionTarget.md) |  | 
**expires_at** | **datetime** | Expiry timestamp (UTC) the listener tears down at. | 
**idle_timeout_seconds** | **int** | Idle window in whole seconds the listener applies locally.  | [optional] 
**callback_url** | **str** | The per-session callback URL under &#x60;/v1/nodes/{id}/sessions/{session_id}&#x60; the Node posts per-kind activity to.  | 

## Example

```python
from plexsphere.models.sse_event_session_setup import SSEEventSessionSetup

# TODO update the JSON string below
json = "{}"
# create an instance of SSEEventSessionSetup from a JSON string
sse_event_session_setup_instance = SSEEventSessionSetup.from_json(json)
# print the JSON string representation of the object
print(SSEEventSessionSetup.to_json())

# convert the object into a dict
sse_event_session_setup_dict = sse_event_session_setup_instance.to_dict()
# create an instance of SSEEventSessionSetup from a dict
sse_event_session_setup_from_dict = SSEEventSessionSetup.from_dict(sse_event_session_setup_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


