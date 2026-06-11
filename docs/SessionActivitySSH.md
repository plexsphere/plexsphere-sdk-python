# SessionActivitySSH

Per-command SSH activity row a Node posts for an `ssh` session. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**command** | **str** | The command the session executed, captured as raw text bounded at 1 KiB.  | 
**exit_code** | **int** | Process exit code the command reported, when settled. | [optional] 
**started_at** | **datetime** | Timestamp the command started (UTC). | [optional] 
**completed_at** | **datetime** | Timestamp the command completed (UTC), when settled. | [optional] 

## Example

```python
from plexsphere.models.session_activity_ssh import SessionActivitySSH

# TODO update the JSON string below
json = "{}"
# create an instance of SessionActivitySSH from a JSON string
session_activity_ssh_instance = SessionActivitySSH.from_json(json)
# print the JSON string representation of the object
print(SessionActivitySSH.to_json())

# convert the object into a dict
session_activity_ssh_dict = session_activity_ssh_instance.to_dict()
# create an instance of SessionActivitySSH from a dict
session_activity_ssh_from_dict = SessionActivitySSH.from_dict(session_activity_ssh_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


