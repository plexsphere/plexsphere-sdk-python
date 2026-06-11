# IssuedSession

Body of a successful `IssueSession`. Carries the metadata-only Session projection, the signed EdDSA JWT (delivered EXACTLY ONCE, here — it is never persisted server-side beyond the identifier and expiry, and never re-exposed through the list or read projections), the per-kind plexd listener endpoint the operator's client connects to, and the expiry. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**session_id** | **UUID** | Identifier of the issued Session (UUIDv7). Equals the token&#39;s &#x60;jti&#x60;.  | 
**token** | **str** | The signed, session-scoped EdDSA JWT. Delivered exactly once, in this response. Treat as a bearer secret.  | 
**expires_at** | **datetime** | Expiry timestamp (UTC) of the issued Session and its token.  | 
**listener_endpoint** | **str** | The on-Node endpoint (&#x60;host:port&#x60;) the operator&#39;s client connects to for this session kind.  | [optional] 
**session** | [**Session**](Session.md) |  | 

## Example

```python
from plexsphere.models.issued_session import IssuedSession

# TODO update the JSON string below
json = "{}"
# create an instance of IssuedSession from a JSON string
issued_session_instance = IssuedSession.from_json(json)
# print the JSON string representation of the object
print(IssuedSession.to_json())

# convert the object into a dict
issued_session_dict = issued_session_instance.to_dict()
# create an instance of IssuedSession from a dict
issued_session_from_dict = IssuedSession.from_dict(issued_session_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


