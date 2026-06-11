# BridgeSiteToSiteTunnelResponse

Hydrated projection of a bridge site-to-site tunnel. Shared by `CreateBridgeSiteToSiteTunnel`, `GetBridgeSiteToSiteTunnel`, `UpdateBridgeSiteToSiteTunnel`, and the list wrapper. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**id** | **UUID** | Tunnel identifier (UUIDv7). | 
**slug** | **str** | Stable identity of the tunnel within its Resource. | 
**kind** | [**BridgeSiteToSiteTunnelKind**](BridgeSiteToSiteTunnelKind.md) |  | 
**remote_host** | **str** | Hostname or address of the remote tunnel endpoint. | 
**remote_port** | **int** | Port on the remote tunnel endpoint. | 
**auth_secret_ref** | **str** | Opaque reference to the tunnel&#39;s authentication material in the form &#x60;secret:&lt;domain&gt;/&lt;project&gt;/&lt;name&gt;(:&lt;version&gt;)?&#x60;.  | 
**allowed_subnets** | **List[str]** | CIDR prefixes the tunnel routes. | 
**routing_policy** | [**BridgeSiteToSiteTunnelRoutingPolicy**](BridgeSiteToSiteTunnelRoutingPolicy.md) |  | 
**created_at** | **datetime** | Tunnel creation timestamp (UTC). | 
**updated_at** | **datetime** | Last-modified timestamp (UTC). | 

## Example

```python
from plexsphere.models.bridge_site_to_site_tunnel_response import BridgeSiteToSiteTunnelResponse

# TODO update the JSON string below
json = "{}"
# create an instance of BridgeSiteToSiteTunnelResponse from a JSON string
bridge_site_to_site_tunnel_response_instance = BridgeSiteToSiteTunnelResponse.from_json(json)
# print the JSON string representation of the object
print(BridgeSiteToSiteTunnelResponse.to_json())

# convert the object into a dict
bridge_site_to_site_tunnel_response_dict = bridge_site_to_site_tunnel_response_instance.to_dict()
# create an instance of BridgeSiteToSiteTunnelResponse from a dict
bridge_site_to_site_tunnel_response_from_dict = BridgeSiteToSiteTunnelResponse.from_dict(bridge_site_to_site_tunnel_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


