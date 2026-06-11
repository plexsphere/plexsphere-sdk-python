# SessionActivityRequest

Body a Node POSTs to `POST /v1/nodes/{id}/sessions/{session_id}` to report per-kind session activity. EXACTLY ONE of `ssh`, `k8s`, or `tcp` is populated, matching the addressed Session's kind. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**ssh** | [**SessionActivitySSH**](SessionActivitySSH.md) | Populated only for an &#x60;ssh&#x60; session. | [optional] 
**k8s** | [**SessionActivityK8s**](SessionActivityK8s.md) | Populated only for a &#x60;k8s&#x60; session. | [optional] 
**tcp** | [**SessionActivityTCP**](SessionActivityTCP.md) | Populated only for a &#x60;tcp&#x60; session. | [optional] 

## Example

```python
from plexsphere.models.session_activity_request import SessionActivityRequest

# TODO update the JSON string below
json = "{}"
# create an instance of SessionActivityRequest from a JSON string
session_activity_request_instance = SessionActivityRequest.from_json(json)
# print the JSON string representation of the object
print(SessionActivityRequest.to_json())

# convert the object into a dict
session_activity_request_dict = session_activity_request_instance.to_dict()
# create an instance of SessionActivityRequest from a dict
session_activity_request_from_dict = SessionActivityRequest.from_dict(session_activity_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


