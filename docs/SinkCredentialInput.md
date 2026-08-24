# SinkCredentialInput

The credential coordinates a caller states when declaring or replacing a tenant sink. There is deliberately no path field: the path is always derived server-side from the owning Domain and the sink id. The body never carries the secret value itself. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**kv_mount** | **str** | KV mount the material lives under. Free of surrounding whitespace and of any &#x60;..&#x60; traversal segment.  | 
**kv_version** | **int** | KV version to read. Absent or &#x60;0&#x60; reads the latest one.  | [optional] 

## Example

```python
from plexsphere.models.sink_credential_input import SinkCredentialInput

# TODO update the JSON string below
json = "{}"
# create an instance of SinkCredentialInput from a JSON string
sink_credential_input_instance = SinkCredentialInput.from_json(json)
# print the JSON string representation of the object
print(SinkCredentialInput.to_json())

# convert the object into a dict
sink_credential_input_dict = sink_credential_input_instance.to_dict()
# create an instance of SinkCredentialInput from a dict
sink_credential_input_from_dict = SinkCredentialInput.from_dict(sink_credential_input_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


