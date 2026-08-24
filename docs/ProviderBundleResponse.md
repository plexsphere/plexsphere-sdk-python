# ProviderBundleResponse

Hydrated projection of a `ProviderBundle` aggregate. The shape is shared by `CreateProviderBundle`, `GetProviderBundle`, `ListProviderBundles`, and `PatchProviderBundle` — every read surface returns the same wire bytes for the same bundle so clients only need one binding.  The optimistic-concurrency counter the aggregate carries is deliberately absent: a patch is gated on the version the server read, not on one the client echoes back, so the counter never crosses the wire. `latest_version` is a different number and IS on the wire: it names the newest declaration the bundle has published, which is the value a Cloud pins. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**id** | **UUID** | Provider bundle identifier (UUIDv7). | 
**slug** | **str** | Kebab-case URL handle. Stable for the lifetime of the bundle — see &#x60;PatchProviderBundle&#x60; for the immutability rationale.  | 
**display_name** | **str** | Human-readable bundle name (trimmed at the aggregate). | 
**provider** | [**CloudProvider**](CloudProvider.md) |  | 
**provider_packages** | [**List[CloudProviderPackage]**](CloudProviderPackage.md) | The Crossplane provider packages this bundle declares, rendered in canonical source-ascending order regardless of the order the operator stated them in.  | 
**provider_config_api_version** | **str** | The &#x60;&lt;group&gt;/&lt;version&gt;&#x60; every declared package serves its ProviderConfig under (e.g. &#x60;aws.m.upbound.io/v1beta1&#x60;). A Cloud pinned to the latest version resolves to this value.  | 
**latest_version** | **int** | The newest content version the bundle has published. A content patch — one carrying &#x60;provider_packages&#x60; or &#x60;provider_config_api_version&#x60; — publishes the next one and reports it here; a rename-only patch leaves it where it is. The two content fields publish ONE version between them even when a patch names both. This is the version an attach with no explicit &#x60;provider_bundle_version&#x60; pins, and the whole history is readable at &#x60;GET /v1/provider-bundles/{id}/versions&#x60;.  | 
**created_at** | **datetime** | Aggregate creation timestamp (UTC). | 
**updated_at** | **datetime** | Last-modified timestamp (UTC). Bumped by every mutator — &#x60;Rename&#x60;, &#x60;ChangeProviderPackages&#x60;, &#x60;ChangeProviderConfigAPIVersion&#x60;.  | 

## Example

```python
from plexsphere.models.provider_bundle_response import ProviderBundleResponse

# TODO update the JSON string below
json = "{}"
# create an instance of ProviderBundleResponse from a JSON string
provider_bundle_response_instance = ProviderBundleResponse.from_json(json)
# print the JSON string representation of the object
print(ProviderBundleResponse.to_json())

# convert the object into a dict
provider_bundle_response_dict = provider_bundle_response_instance.to_dict()
# create an instance of ProviderBundleResponse from a dict
provider_bundle_response_from_dict = ProviderBundleResponse.from_dict(provider_bundle_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


