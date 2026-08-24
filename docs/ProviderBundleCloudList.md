# ProviderBundleCloudList

Page of the Clouds referencing one provider bundle, returned by `GET /v1/provider-bundles/{id}/clouds`. The window is computed by the persistence layer in `cloud_id` order. Unlike the provider-bundle and Cloud collections, the `items` array is not narrowed by a per-row visibility filter: it is the whole set of Clouds the addressed bundle configures. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**items** | [**List[ProviderBundleCloudRef]**](ProviderBundleCloudRef.md) | Clouds referencing the bundle in the current page.  | 
**next_cursor** | **str** | Continuation token for the next page. Absent when the iteration has reached end-of-stream. The encoding is HMAC-signed by the server so a tampered cursor surfaces as &#x60;400&#x60; on the next call.  | [optional] 

## Example

```python
from plexsphere.models.provider_bundle_cloud_list import ProviderBundleCloudList

# TODO update the JSON string below
json = "{}"
# create an instance of ProviderBundleCloudList from a JSON string
provider_bundle_cloud_list_instance = ProviderBundleCloudList.from_json(json)
# print the JSON string representation of the object
print(ProviderBundleCloudList.to_json())

# convert the object into a dict
provider_bundle_cloud_list_dict = provider_bundle_cloud_list_instance.to_dict()
# create an instance of ProviderBundleCloudList from a dict
provider_bundle_cloud_list_from_dict = ProviderBundleCloudList.from_dict(provider_bundle_cloud_list_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


