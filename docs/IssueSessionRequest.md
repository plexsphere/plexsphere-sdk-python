# IssueSessionRequest

Body for `POST /v1/projects/{project_id}/sessions`. It names the targeted Resource, the session kind, the matching per-kind target, and the optional TTL and idempotency key. The `target` must carry EXACTLY the variant that matches `kind`. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**resource_id** | **UUID** | Identifier of the Resource to open a session against. | 
**kind** | [**SessionKind**](SessionKind.md) |  | 
**target** | [**SessionTarget**](SessionTarget.md) |  | 
**requested_ttl_seconds** | **int** | Operator-requested TTL in whole seconds. Omitted falls back to the per-Domain default; a value above the per-Domain maximum is silently clamped, not rejected — the response &#x60;expires_at&#x60; reflects the effective TTL.  | [optional] 
**idempotency_key** | **str** | Optional idempotency key. A repeat issuance with the same &#x60;(identity, idempotency_key)&#x60; within the dedupe window returns the original Session and token rather than minting a new one.  | [optional] 

## Example

```python
from plexsphere.models.issue_session_request import IssueSessionRequest

# TODO update the JSON string below
json = "{}"
# create an instance of IssueSessionRequest from a JSON string
issue_session_request_instance = IssueSessionRequest.from_json(json)
# print the JSON string representation of the object
print(IssueSessionRequest.to_json())

# convert the object into a dict
issue_session_request_dict = issue_session_request_instance.to_dict()
# create an instance of IssueSessionRequest from a dict
issue_session_request_from_dict = IssueSessionRequest.from_dict(issue_session_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


