# ProviderBundleCreateRequest

Body for `POST /v1/provider-bundles`. The field set mirrors the `ProviderBundle` aggregate's construction invariants. The handler authorises the call against the platform-level `manage` relation BEFORE invoking the service, so an unauthorised caller never produces a `ProviderBundleCreated` outbox row. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**display_name** | **str** | Human-readable bundle name. Whitespace-only is rejected. | 
**slug** | **str** | Kebab-case URL handle. The aggregate&#39;s &#x60;ParseSlug&#x60; enforces the same regex; surfacing the pattern here lets the generated client validate before the round-trip.  | 
**provider** | [**CloudProvider**](CloudProvider.md) |  | 
**provider_packages** | [**List[CloudProviderPackage]**](CloudProviderPackage.md) | The Crossplane provider packages the bundle declares. At least one entry is required, and each &#x60;source&#x60; may appear only once. The read surfaces render the set in canonical source-ascending order, not in the order stated here.  | 
**provider_config_api_version** | **str** | The &#x60;&lt;group&gt;/&lt;version&gt;&#x60; every declared package serves its ProviderConfig under (e.g. &#x60;aws.m.upbound.io/v1beta1&#x60;).  | 

## Example

```python
from plexsphere.models.provider_bundle_create_request import ProviderBundleCreateRequest

# TODO update the JSON string below
json = "{}"
# create an instance of ProviderBundleCreateRequest from a JSON string
provider_bundle_create_request_instance = ProviderBundleCreateRequest.from_json(json)
# print the JSON string representation of the object
print(ProviderBundleCreateRequest.to_json())

# convert the object into a dict
provider_bundle_create_request_dict = provider_bundle_create_request_instance.to_dict()
# create an instance of ProviderBundleCreateRequest from a dict
provider_bundle_create_request_from_dict = ProviderBundleCreateRequest.from_dict(provider_bundle_create_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


