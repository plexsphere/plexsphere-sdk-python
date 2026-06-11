# BridgeSiteToSiteTunnelUpdateRequest

Body for `UpdateBridgeSiteToSiteTunnel`. Full replacement of the tunnel's mutable configuration — not a partial merge. The slug is the path identity and is not carried in the body. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**kind** | [**BridgeSiteToSiteTunnelKind**](BridgeSiteToSiteTunnelKind.md) |  | 
**remote_host** | **str** | Hostname or address of the remote tunnel endpoint. | 
**remote_port** | **int** | Port on the remote tunnel endpoint. Outside &#x60;1..65535&#x60; the write is rejected with &#x60;400 relay_port_out_of_range&#x60;.  | 
**auth_secret_ref** | **str** | Opaque reference to the tunnel&#39;s authentication material in the form &#x60;secret:&lt;domain&gt;/&lt;project&gt;/&lt;name&gt;(:&lt;version&gt;)?&#x60;.  | 
**allowed_subnets** | **List[str]** | CIDR prefixes the tunnel routes. An empty list surfaces as &#x60;400 allowed_subnet_empty&#x60;.  | 
**routing_policy** | [**BridgeSiteToSiteTunnelRoutingPolicy**](BridgeSiteToSiteTunnelRoutingPolicy.md) |  | 

## Example

```python
from plexsphere.models.bridge_site_to_site_tunnel_update_request import BridgeSiteToSiteTunnelUpdateRequest

# TODO update the JSON string below
json = "{}"
# create an instance of BridgeSiteToSiteTunnelUpdateRequest from a JSON string
bridge_site_to_site_tunnel_update_request_instance = BridgeSiteToSiteTunnelUpdateRequest.from_json(json)
# print the JSON string representation of the object
print(BridgeSiteToSiteTunnelUpdateRequest.to_json())

# convert the object into a dict
bridge_site_to_site_tunnel_update_request_dict = bridge_site_to_site_tunnel_update_request_instance.to_dict()
# create an instance of BridgeSiteToSiteTunnelUpdateRequest from a dict
bridge_site_to_site_tunnel_update_request_from_dict = BridgeSiteToSiteTunnelUpdateRequest.from_dict(bridge_site_to_site_tunnel_update_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


