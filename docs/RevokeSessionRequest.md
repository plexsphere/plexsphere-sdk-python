# RevokeSessionRequest

Body for `POST /v1/projects/{project_id}/sessions/{session_id}:revoke`. Carries the operator's revocation reason. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**revoke_reason** | [**RevokeReason**](RevokeReason.md) | Reason for the revocation. Only the operator-driven values are accepted here — the sweeper reasons (&#x60;ttl_expired&#x60;, &#x60;idle_timeout&#x60;) are set internally, never by this call.  | 

## Example

```python
from plexsphere.models.revoke_session_request import RevokeSessionRequest

# TODO update the JSON string below
json = "{}"
# create an instance of RevokeSessionRequest from a JSON string
revoke_session_request_instance = RevokeSessionRequest.from_json(json)
# print the JSON string representation of the object
print(RevokeSessionRequest.to_json())

# convert the object into a dict
revoke_session_request_dict = revoke_session_request_instance.to_dict()
# create an instance of RevokeSessionRequest from a dict
revoke_session_request_from_dict = RevokeSessionRequest.from_dict(revoke_session_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


