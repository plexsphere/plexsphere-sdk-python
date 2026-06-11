# BridgeSiteToSiteTunnelList

Site-to-site tunnels on a bridge Resource, returned by `ListBridgeSiteToSiteTunnels`. Ordered by slug ascending. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**tunnels** | [**List[BridgeSiteToSiteTunnelResponse]**](BridgeSiteToSiteTunnelResponse.md) | Tunnels ordered by slug ascending. | 

## Example

```python
from plexsphere.models.bridge_site_to_site_tunnel_list import BridgeSiteToSiteTunnelList

# TODO update the JSON string below
json = "{}"
# create an instance of BridgeSiteToSiteTunnelList from a JSON string
bridge_site_to_site_tunnel_list_instance = BridgeSiteToSiteTunnelList.from_json(json)
# print the JSON string representation of the object
print(BridgeSiteToSiteTunnelList.to_json())

# convert the object into a dict
bridge_site_to_site_tunnel_list_dict = bridge_site_to_site_tunnel_list_instance.to_dict()
# create an instance of BridgeSiteToSiteTunnelList from a dict
bridge_site_to_site_tunnel_list_from_dict = BridgeSiteToSiteTunnelList.from_dict(bridge_site_to_site_tunnel_list_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


