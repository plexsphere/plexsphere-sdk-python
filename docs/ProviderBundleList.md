# ProviderBundleList

Page of provider bundles returned by `GET /v1/provider-bundles`. The window is computed by the persistence layer in slug order; per-row visibility is layered on top so the `items` array is the subset the caller is authorised to see. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**items** | [**List[ProviderBundleResponse]**](ProviderBundleResponse.md) | Provider bundles in the current page. | 
**next_cursor** | **str** | Continuation token for the next page. Absent when the iteration has reached end-of-stream. The encoding is HMAC-signed by the server so a tampered cursor surfaces as &#x60;400&#x60; on the next call.  | [optional] 

## Example

```python
from plexsphere.models.provider_bundle_list import ProviderBundleList

# TODO update the JSON string below
json = "{}"
# create an instance of ProviderBundleList from a JSON string
provider_bundle_list_instance = ProviderBundleList.from_json(json)
# print the JSON string representation of the object
print(ProviderBundleList.to_json())

# convert the object into a dict
provider_bundle_list_dict = provider_bundle_list_instance.to_dict()
# create an instance of ProviderBundleList from a dict
provider_bundle_list_from_dict = ProviderBundleList.from_dict(provider_bundle_list_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


