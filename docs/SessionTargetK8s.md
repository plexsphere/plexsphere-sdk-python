# SessionTargetK8s

Kubernetes session target — the impersonated user and the optional impersonation groups the mediated kubectl session asserts. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**user** | **str** | User the mediated Kubernetes session impersonates. | 
**impersonate_groups** | **List[str]** | Optional impersonation groups the session asserts. Bounded at 32 entries.  | [optional] 

## Example

```python
from plexsphere.models.session_target_k8s import SessionTargetK8s

# TODO update the JSON string below
json = "{}"
# create an instance of SessionTargetK8s from a JSON string
session_target_k8s_instance = SessionTargetK8s.from_json(json)
# print the JSON string representation of the object
print(SessionTargetK8s.to_json())

# convert the object into a dict
session_target_k8s_dict = session_target_k8s_instance.to_dict()
# create an instance of SessionTargetK8s from a dict
session_target_k8s_from_dict = SessionTargetK8s.from_dict(session_target_k8s_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


