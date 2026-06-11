# NodeStateBridgeUserAccess

Per-Node projection of the bridge user-access providers the Node must program. Reuses the `BridgeUserAccessProviderResponse` wire shape so the delivered config matches the management-surface bytes for the same provider. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**providers** | [**List[BridgeUserAccessProviderResponse]**](BridgeUserAccessProviderResponse.md) | User-access providers ordered by slug ascending. | 

## Example

```python
from plexsphere.models.node_state_bridge_user_access import NodeStateBridgeUserAccess

# TODO update the JSON string below
json = "{}"
# create an instance of NodeStateBridgeUserAccess from a JSON string
node_state_bridge_user_access_instance = NodeStateBridgeUserAccess.from_json(json)
# print the JSON string representation of the object
print(NodeStateBridgeUserAccess.to_json())

# convert the object into a dict
node_state_bridge_user_access_dict = node_state_bridge_user_access_instance.to_dict()
# create an instance of NodeStateBridgeUserAccess from a dict
node_state_bridge_user_access_from_dict = NodeStateBridgeUserAccess.from_dict(node_state_bridge_user_access_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


