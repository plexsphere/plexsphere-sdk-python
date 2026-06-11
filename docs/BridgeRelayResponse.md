# BridgeRelayResponse

Hydrated projection of the per-Resource relay singleton. Returned by `GetBridgeRelay` and `ConfigureBridgeRelay`. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**resource_id** | **UUID** | Owning bridge Resource (UUIDv7) — the relay primary key. | 
**enabled** | **bool** | Whether the relay is active. | 
**listen_port** | **int** | UDP port the relay listens on. | 
**created_at** | **datetime** | Relay creation timestamp (UTC). | 
**updated_at** | **datetime** | Last-modified timestamp (UTC). | 

## Example

```python
from plexsphere.models.bridge_relay_response import BridgeRelayResponse

# TODO update the JSON string below
json = "{}"
# create an instance of BridgeRelayResponse from a JSON string
bridge_relay_response_instance = BridgeRelayResponse.from_json(json)
# print the JSON string representation of the object
print(BridgeRelayResponse.to_json())

# convert the object into a dict
bridge_relay_response_dict = bridge_relay_response_instance.to_dict()
# create an instance of BridgeRelayResponse from a dict
bridge_relay_response_from_dict = BridgeRelayResponse.from_dict(bridge_relay_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


