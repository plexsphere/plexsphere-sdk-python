# SSEEventSessionRevoked

Payload of the `session_revoked` Signed Event Bus event delivered to the target Node's event stream (and to any operator-side subscriber). The Node tears the listener down on receipt. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**session_id** | **UUID** | Identifier of the revoked Session (UUIDv7). | 
**jti** | **str** | The revoked token&#39;s &#x60;jti&#x60;. Equals the session identifier.  | 
**revoked_at** | **datetime** | Revocation timestamp (UTC). | 
**revoke_reason** | [**RevokeReason**](RevokeReason.md) |  | 

## Example

```python
from plexsphere.models.sse_event_session_revoked import SSEEventSessionRevoked

# TODO update the JSON string below
json = "{}"
# create an instance of SSEEventSessionRevoked from a JSON string
sse_event_session_revoked_instance = SSEEventSessionRevoked.from_json(json)
# print the JSON string representation of the object
print(SSEEventSessionRevoked.to_json())

# convert the object into a dict
sse_event_session_revoked_dict = sse_event_session_revoked_instance.to_dict()
# create an instance of SSEEventSessionRevoked from a dict
sse_event_session_revoked_from_dict = SSEEventSessionRevoked.from_dict(sse_event_session_revoked_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


