# Session

Metadata-only projection of a mediated Session. The shape is shared by `IssueSession` (nested in the issue response), `ListSessions`, and `GetSession`. It carries the session identity, the owning Project / Domain / Resource / Identity, the aggregate status, the issuance / expiry / last-active timestamps, and the per-kind target — but NEVER the signed token. The token is delivered exactly once, in the issue response, and never re-exposed through this projection. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**id** | **UUID** | Session identifier (UUIDv7). Equals the issued token&#39;s &#x60;jti&#x60;.  | 
**project_id** | **UUID** | Identifier of the owning Project (UUIDv7). | 
**domain_id** | **UUID** | Identifier of the owning Domain (UUIDv7) — the residency pivot the per-Domain session policy and concurrency budgets count against.  | 
**resource_id** | **UUID** | Identifier of the targeted Resource (UUIDv7). | 
**identity_id** | **UUID** | Identifier of the Identity the Session was issued to. | 
**kind** | [**SessionKind**](SessionKind.md) |  | 
**status** | [**SessionStatus**](SessionStatus.md) | Aggregate lifecycle status of the Session. | 
**issued_at** | **datetime** | Issuance timestamp (UTC). | 
**expires_at** | **datetime** | Expiry timestamp (UTC) — issuance time plus the clamped TTL.  | 
**last_active_at** | **datetime** | Timestamp of the most recent recorded activity (UTC); drives the idle-timeout sweeper.  | 
**idle_timeout_seconds** | **int** | Idle window in whole seconds — the Session is reclaimed when &#x60;last_active_at + idle_timeout&#x60; passes.  | [optional] 
**revoked_at** | **datetime** | Revocation timestamp (UTC); &#x60;null&#x60; while the Session is live.  | [optional] 
**revoke_reason** | [**RevokeReason**](RevokeReason.md) | Reason the Session was revoked. Omitted while the Session is live.  | [optional] 
**target** | [**SessionTarget**](SessionTarget.md) |  | 

## Example

```python
from plexsphere.models.session import Session

# TODO update the JSON string below
json = "{}"
# create an instance of Session from a JSON string
session_instance = Session.from_json(json)
# print the JSON string representation of the object
print(Session.to_json())

# convert the object into a dict
session_dict = session_instance.to_dict()
# create an instance of Session from a dict
session_from_dict = Session.from_dict(session_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


