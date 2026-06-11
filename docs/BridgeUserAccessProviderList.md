# BridgeUserAccessProviderList

User-access providers on a bridge Resource, returned by `ListBridgeUserAccessProviders`. Ordered by slug ascending. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**providers** | [**List[BridgeUserAccessProviderResponse]**](BridgeUserAccessProviderResponse.md) | Providers ordered by slug ascending. | 

## Example

```python
from plexsphere.models.bridge_user_access_provider_list import BridgeUserAccessProviderList

# TODO update the JSON string below
json = "{}"
# create an instance of BridgeUserAccessProviderList from a JSON string
bridge_user_access_provider_list_instance = BridgeUserAccessProviderList.from_json(json)
# print the JSON string representation of the object
print(BridgeUserAccessProviderList.to_json())

# convert the object into a dict
bridge_user_access_provider_list_dict = bridge_user_access_provider_list_instance.to_dict()
# create an instance of BridgeUserAccessProviderList from a dict
bridge_user_access_provider_list_from_dict = BridgeUserAccessProviderList.from_dict(bridge_user_access_provider_list_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


