# NodeStateSession

A single live mediated session projected onto the `NodeStateSnapshot` envelope. It mirrors the `session_setup` Signed Event Bus payload minus the callback URL: the agent derives its per-session callback path from its configured base per the agent contract. The Node opens its on-node listener for the session from this projection. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**session_id** | **UUID** | Identifier of the Session (UUIDv7). | 
**jti** | **str** | The issued token&#39;s &#x60;jti&#x60;. Equals the session identifier.  | 
**kind** | [**SessionKind**](SessionKind.md) |  | 
**target** | [**SessionTarget**](SessionTarget.md) |  | 
**expires_at** | **datetime** | Expiry timestamp (UTC) the listener tears down at. | 
**idle_timeout_seconds** | **int** | Idle window in whole seconds the listener applies locally.  | [optional] 

## Example

```python
from plexsphere.models.node_state_session import NodeStateSession

# TODO update the JSON string below
json = "{}"
# create an instance of NodeStateSession from a JSON string
node_state_session_instance = NodeStateSession.from_json(json)
# print the JSON string representation of the object
print(NodeStateSession.to_json())

# convert the object into a dict
node_state_session_dict = node_state_session_instance.to_dict()
# create an instance of NodeStateSession from a dict
node_state_session_from_dict = NodeStateSession.from_dict(node_state_session_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


