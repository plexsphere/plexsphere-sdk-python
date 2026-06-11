# SessionActivityK8s

Per-request Kubernetes activity row a Node posts for a `k8s` session. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**verb** | **str** | Kubernetes API verb the request issued. | 
**resource_kind** | **str** | Kubernetes resource kind the request addressed. | [optional] 
**namespace** | **str** | Namespace the request addressed, when namespaced. | [optional] 
**name** | **str** | Resource name the request addressed, when named. | [optional] 
**status_code** | **int** | HTTP status the API server returned. | [optional] 
**duration_ms** | **int** | Request duration in milliseconds. | [optional] 

## Example

```python
from plexsphere.models.session_activity_k8s import SessionActivityK8s

# TODO update the JSON string below
json = "{}"
# create an instance of SessionActivityK8s from a JSON string
session_activity_k8s_instance = SessionActivityK8s.from_json(json)
# print the JSON string representation of the object
print(SessionActivityK8s.to_json())

# convert the object into a dict
session_activity_k8s_dict = session_activity_k8s_instance.to_dict()
# create an instance of SessionActivityK8s from a dict
session_activity_k8s_from_dict = SessionActivityK8s.from_dict(session_activity_k8s_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


