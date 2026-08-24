# SinkCredential

Where a tenant sink's connection material lives. The projection carries the KV coordinates only — never the material itself, which stays in the secret store and is read at delivery time. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**kv_mount** | **str** | KV mount the material lives under. | 
**kv_path** | **str** | Path the material lives at under the mount. It is derived server-side from the owning Domain and the sink id, so a reference can never address another tenant&#39;s material.  | 
**kv_version** | **int** | KV version to read. Absent or &#x60;0&#x60; reads the latest one.  | [optional] 

## Example

```python
from plexsphere.models.sink_credential import SinkCredential

# TODO update the JSON string below
json = "{}"
# create an instance of SinkCredential from a JSON string
sink_credential_instance = SinkCredential.from_json(json)
# print the JSON string representation of the object
print(SinkCredential.to_json())

# convert the object into a dict
sink_credential_dict = sink_credential_instance.to_dict()
# create an instance of SinkCredential from a dict
sink_credential_from_dict = SinkCredential.from_dict(sink_credential_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


