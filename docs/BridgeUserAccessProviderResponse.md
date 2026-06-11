# BridgeUserAccessProviderResponse

Hydrated projection of a bridge user-access provider. Shared by `CreateBridgeUserAccessProvider`, `GetBridgeUserAccessProvider`, `UpdateBridgeUserAccessProvider`, and the list wrapper. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**id** | **UUID** | Provider identifier (UUIDv7). | 
**slug** | **str** | Stable identity of the provider within its Resource. | 
**kind** | [**BridgeUserAccessProviderKind**](BridgeUserAccessProviderKind.md) |  | 
**interface_name** | **str** | Name of the network interface the provider programs on the Node. | 
**listen_port** | **int** | UDP port the provider listens on. | 
**max_peers** | **int** | Maximum number of peers the provider admits. | 
**auth_secret_ref** | **str** | Opaque reference to the provider&#39;s authentication material in the form &#x60;secret:&lt;domain&gt;/&lt;project&gt;/&lt;name&gt;(:&lt;version&gt;)?&#x60;.  | 
**routing_policy** | **Dict[str, object]** | Free-form JSON routing-policy document. | 
**created_at** | **datetime** | Provider creation timestamp (UTC). | 
**updated_at** | **datetime** | Last-modified timestamp (UTC). | 

## Example

```python
from plexsphere.models.bridge_user_access_provider_response import BridgeUserAccessProviderResponse

# TODO update the JSON string below
json = "{}"
# create an instance of BridgeUserAccessProviderResponse from a JSON string
bridge_user_access_provider_response_instance = BridgeUserAccessProviderResponse.from_json(json)
# print the JSON string representation of the object
print(BridgeUserAccessProviderResponse.to_json())

# convert the object into a dict
bridge_user_access_provider_response_dict = bridge_user_access_provider_response_instance.to_dict()
# create an instance of BridgeUserAccessProviderResponse from a dict
bridge_user_access_provider_response_from_dict = BridgeUserAccessProviderResponse.from_dict(bridge_user_access_provider_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


