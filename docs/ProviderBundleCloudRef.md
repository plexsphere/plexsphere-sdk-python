# ProviderBundleCloudRef

Reference to one Cloud that takes its provider configuration from a provider bundle. The item shape of `ProviderBundleCloudList`. The slug and the display name travel alongside the id for the reason `provider_bundle_slug` travels alongside `provider_bundle_id` on `CloudResponse`: the client renders the row without a second round-trip per Cloud. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**cloud_id** | **UUID** | Identifier of the referencing Cloud (UUIDv7). | 
**slug** | **str** | Kebab-case URL handle of the referencing Cloud. | 
**display_name** | **str** | Human-readable name of the referencing Cloud. | 
**provider_bundle_version** | **int** | The content version of the bundle this Cloud pins. Rows on one roster page may name different versions: a bundle patch publishes a new version and moves no Cloud, so each Cloud keeps the declaration it was last pinned to until a Cloud write moves that pin.  | 

## Example

```python
from plexsphere.models.provider_bundle_cloud_ref import ProviderBundleCloudRef

# TODO update the JSON string below
json = "{}"
# create an instance of ProviderBundleCloudRef from a JSON string
provider_bundle_cloud_ref_instance = ProviderBundleCloudRef.from_json(json)
# print the JSON string representation of the object
print(ProviderBundleCloudRef.to_json())

# convert the object into a dict
provider_bundle_cloud_ref_dict = provider_bundle_cloud_ref_instance.to_dict()
# create an instance of ProviderBundleCloudRef from a dict
provider_bundle_cloud_ref_from_dict = ProviderBundleCloudRef.from_dict(provider_bundle_cloud_ref_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


