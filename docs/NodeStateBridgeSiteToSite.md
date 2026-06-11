# NodeStateBridgeSiteToSite

Per-Node projection of the bridge site-to-site tunnels the Node must program. Reuses the `BridgeSiteToSiteTunnelResponse` wire shape so the delivered config matches the management-surface bytes for the same tunnel. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**tunnels** | [**List[BridgeSiteToSiteTunnelResponse]**](BridgeSiteToSiteTunnelResponse.md) | Site-to-site tunnels ordered by slug ascending. | 

## Example

```python
from plexsphere.models.node_state_bridge_site_to_site import NodeStateBridgeSiteToSite

# TODO update the JSON string below
json = "{}"
# create an instance of NodeStateBridgeSiteToSite from a JSON string
node_state_bridge_site_to_site_instance = NodeStateBridgeSiteToSite.from_json(json)
# print the JSON string representation of the object
print(NodeStateBridgeSiteToSite.to_json())

# convert the object into a dict
node_state_bridge_site_to_site_dict = node_state_bridge_site_to_site_instance.to_dict()
# create an instance of NodeStateBridgeSiteToSite from a dict
node_state_bridge_site_to_site_from_dict = NodeStateBridgeSiteToSite.from_dict(node_state_bridge_site_to_site_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


