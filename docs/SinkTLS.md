# SinkTLS

Transport posture a tenant sink connects under. Absent means the system trust store with verification on, which is what a sink delivering to a publicly-trusted endpoint carries. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**ca_pem** | **str** | PEM bundle the destination&#39;s certificate is verified against. Absent or empty selects the system trust store. A non-empty value decodes as PEM and holds at least one certificate.  | [optional] 
**insecure_skip_verify** | **bool** | Disable certificate verification altogether. Intended for a destination presenting a certificate the platform cannot be given a bundle for; it removes the guarantee that the telemetry reaches the endpoint it names.  | [optional] 

## Example

```python
from plexsphere.models.sink_tls import SinkTLS

# TODO update the JSON string below
json = "{}"
# create an instance of SinkTLS from a JSON string
sink_tls_instance = SinkTLS.from_json(json)
# print the JSON string representation of the object
print(SinkTLS.to_json())

# convert the object into a dict
sink_tls_dict = sink_tls_instance.to_dict()
# create an instance of SinkTLS from a dict
sink_tls_from_dict = SinkTLS.from_dict(sink_tls_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


