# ProviderBundlePatchRequest

Body for `PATCH /v1/provider-bundles/{id}`. All properties are optional — but the body MUST set at least one of `display_name`, `provider_packages`, or `provider_config_api_version`. An empty body is rejected at the handler with `400 empty_patch`. A `provider_packages` patch replaces the whole set as one unit: the packages the body names become the bundle's packages and every package it omits is dropped. An empty array, a duplicate `source`, or a package missing one of its two members is rejected with `400 invalid_provider_bundle`.  The immutable `slug` and `provider` are intentionally absent from this schema; the handler rejects a body carrying `slug` with `400 slug_immutable` and one carrying `provider` with `400 provider_immutable`. `provider` is the compatibility key every stored Cloud reference was admitted against, and nothing re-checks a reference once it is stored. See the `PatchProviderBundle` description for the full rationale. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**display_name** | **str** | New human-readable bundle name. The aggregate&#39;s &#x60;Rename&#x60; mutator validates the same constraints as construction.  | [optional] 
**provider_packages** | [**List[CloudProviderPackage]**](CloudProviderPackage.md) | Replacement package set. The whole set is replaced rather than merged. Omit the field to leave the current set untouched. The read surfaces render the result in canonical source-ascending order.  | [optional] 
**provider_config_api_version** | **str** | New &#x60;&lt;group&gt;/&lt;version&gt;&#x60; every declared package serves its ProviderConfig under. Patchable independently of &#x60;provider_packages&#x60;: a package bump within one provider family keeps the served group, and a family migration changes the group without touching the pins.  | [optional] 

## Example

```python
from plexsphere.models.provider_bundle_patch_request import ProviderBundlePatchRequest

# TODO update the JSON string below
json = "{}"
# create an instance of ProviderBundlePatchRequest from a JSON string
provider_bundle_patch_request_instance = ProviderBundlePatchRequest.from_json(json)
# print the JSON string representation of the object
print(ProviderBundlePatchRequest.to_json())

# convert the object into a dict
provider_bundle_patch_request_dict = provider_bundle_patch_request_instance.to_dict()
# create an instance of ProviderBundlePatchRequest from a dict
provider_bundle_patch_request_from_dict = ProviderBundlePatchRequest.from_dict(provider_bundle_patch_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


