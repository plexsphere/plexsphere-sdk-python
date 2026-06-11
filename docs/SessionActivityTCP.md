# SessionActivityTCP

TCP session lifecycle activity a Node posts for a `tcp` session. The shape is deliberately payload-blind — exactly a start and an end with byte counters, no L7 interpretation. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**phase** | **str** | Lifecycle phase: &#x60;session_started&#x60; opens the tunnel, &#x60;session_ended&#x60; closes it.  | 
**target_host** | **str** | Target host the tunnel connected to. Present on &#x60;session_started&#x60;.  | [optional] 
**target_port** | **int** | Target port the tunnel connected to. Present on &#x60;session_started&#x60;.  | [optional] 
**bytes_in** | **int** | Bytes forwarded from the operator to the target. Present on &#x60;session_ended&#x60;.  | [optional] 
**bytes_out** | **int** | Bytes forwarded from the target to the operator. Present on &#x60;session_ended&#x60;.  | [optional] 
**terminated_by** | **str** | What closed the tunnel. Present on &#x60;session_ended&#x60;.  | [optional] 

## Example

```python
from plexsphere.models.session_activity_tcp import SessionActivityTCP

# TODO update the JSON string below
json = "{}"
# create an instance of SessionActivityTCP from a JSON string
session_activity_tcp_instance = SessionActivityTCP.from_json(json)
# print the JSON string representation of the object
print(SessionActivityTCP.to_json())

# convert the object into a dict
session_activity_tcp_dict = session_activity_tcp_instance.to_dict()
# create an instance of SessionActivityTCP from a dict
session_activity_tcp_from_dict = SessionActivityTCP.from_dict(session_activity_tcp_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


