# BridgeRelayConfigureRequest

Full replacement of the per-Resource relay configuration. The relay is a singleton keyed on the Resource; this body replaces it wholesale (last-writer-wins). An identical-config PUT is an idempotent no-op. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**enabled** | **bool** | Whether the relay is active. A disabled relay keeps its configuration but programs no listener on the Node.  | 
**listen_port** | **int** | UDP port the relay listens on. Outside &#x60;1..65535&#x60; the write is rejected with &#x60;400 relay_port_out_of_range&#x60;.  | 

## Example

```python
from plexsphere.models.bridge_relay_configure_request import BridgeRelayConfigureRequest

# TODO update the JSON string below
json = "{}"
# create an instance of BridgeRelayConfigureRequest from a JSON string
bridge_relay_configure_request_instance = BridgeRelayConfigureRequest.from_json(json)
# print the JSON string representation of the object
print(BridgeRelayConfigureRequest.to_json())

# convert the object into a dict
bridge_relay_configure_request_dict = bridge_relay_configure_request_instance.to_dict()
# create an instance of BridgeRelayConfigureRequest from a dict
bridge_relay_configure_request_from_dict = BridgeRelayConfigureRequest.from_dict(bridge_relay_configure_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


