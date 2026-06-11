# SessionTargetTCP

Generic TCP session target — the host and port the protocol-opaque tunnel forwards to. No L7 shape is recorded or interpreted. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**host** | **str** | Target host the tunnel forwards to. | 
**port** | **int** | Target port the tunnel forwards to. | 

## Example

```python
from plexsphere.models.session_target_tcp import SessionTargetTCP

# TODO update the JSON string below
json = "{}"
# create an instance of SessionTargetTCP from a JSON string
session_target_tcp_instance = SessionTargetTCP.from_json(json)
# print the JSON string representation of the object
print(SessionTargetTCP.to_json())

# convert the object into a dict
session_target_tcp_dict = session_target_tcp_instance.to_dict()
# create an instance of SessionTargetTCP from a dict
session_target_tcp_from_dict = SessionTargetTCP.from_dict(session_target_tcp_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


