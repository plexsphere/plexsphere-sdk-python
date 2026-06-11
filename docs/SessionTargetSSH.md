# SessionTargetSSH

SSH session target — the login user and the optional allowed- command allowlist the mediated SSH session is constrained to. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**user** | **str** | Login user the mediated SSH session connects as. | 
**allowed_commands** | **List[str]** | Optional allowlist of commands the session may run. An empty or omitted list grants an unconstrained interactive session; bounded at 64 entries.  | [optional] 

## Example

```python
from plexsphere.models.session_target_ssh import SessionTargetSSH

# TODO update the JSON string below
json = "{}"
# create an instance of SessionTargetSSH from a JSON string
session_target_ssh_instance = SessionTargetSSH.from_json(json)
# print the JSON string representation of the object
print(SessionTargetSSH.to_json())

# convert the object into a dict
session_target_ssh_dict = session_target_ssh_instance.to_dict()
# create an instance of SessionTargetSSH from a dict
session_target_ssh_from_dict = SessionTargetSSH.from_dict(session_target_ssh_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


