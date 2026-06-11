# SessionTarget

Per-kind target of a Session. EXACTLY ONE of `ssh`, `k8s`, or `tcp` is populated, matching the Session's `kind`. The target carries the operator-supplied connection coordinates the plexd listener binds against — it is metadata, not credential material. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**ssh** | [**SessionTargetSSH**](SessionTargetSSH.md) | Populated only when &#x60;kind&#x60; is &#x60;ssh&#x60;.  | [optional] 
**k8s** | [**SessionTargetK8s**](SessionTargetK8s.md) | Populated only when &#x60;kind&#x60; is &#x60;k8s&#x60;.  | [optional] 
**tcp** | [**SessionTargetTCP**](SessionTargetTCP.md) | Populated only when &#x60;kind&#x60; is &#x60;tcp&#x60;.  | [optional] 

## Example

```python
from plexsphere.models.session_target import SessionTarget

# TODO update the JSON string below
json = "{}"
# create an instance of SessionTarget from a JSON string
session_target_instance = SessionTarget.from_json(json)
# print the JSON string representation of the object
print(SessionTarget.to_json())

# convert the object into a dict
session_target_dict = session_target_instance.to_dict()
# create an instance of SessionTarget from a dict
session_target_from_dict = SessionTarget.from_dict(session_target_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


