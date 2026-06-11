# BridgeUserAccessProviderUpdateRequest

Body for `UpdateBridgeUserAccessProvider`. Full replacement of the provider's mutable configuration — not a partial merge. The slug is the path identity and is not carried in the body. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**kind** | [**BridgeUserAccessProviderKind**](BridgeUserAccessProviderKind.md) |  | 
**interface_name** | **str** | Name of the network interface the provider programs on the Node. | 
**listen_port** | **int** | UDP port the provider listens on. Outside &#x60;1..65535&#x60; the write is rejected with &#x60;400 relay_port_out_of_range&#x60;.  | 
**max_peers** | **int** | Maximum number of peers the provider admits. | 
**auth_secret_ref** | **str** | Opaque reference to the provider&#39;s authentication material in the form &#x60;secret:&lt;domain&gt;/&lt;project&gt;/&lt;name&gt;(:&lt;version&gt;)?&#x60;. The platform stores the reference, never the material; a malformed reference surfaces as &#x60;400 secret_ref_malformed&#x60;.  | 
**routing_policy** | **Dict[str, object]** | Free-form JSON routing-policy document, persisted as JSONB.  | 

## Example

```python
from plexsphere.models.bridge_user_access_provider_update_request import BridgeUserAccessProviderUpdateRequest

# TODO update the JSON string below
json = "{}"
# create an instance of BridgeUserAccessProviderUpdateRequest from a JSON string
bridge_user_access_provider_update_request_instance = BridgeUserAccessProviderUpdateRequest.from_json(json)
# print the JSON string representation of the object
print(BridgeUserAccessProviderUpdateRequest.to_json())

# convert the object into a dict
bridge_user_access_provider_update_request_dict = bridge_user_access_provider_update_request_instance.to_dict()
# create an instance of BridgeUserAccessProviderUpdateRequest from a dict
bridge_user_access_provider_update_request_from_dict = BridgeUserAccessProviderUpdateRequest.from_dict(bridge_user_access_provider_update_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


