# ProviderBundleVersion

One published declaration of a provider bundle: the package set and the ProviderConfig apiVersion frozen under one version number. The item shape of `ProviderBundleVersionList`. A version is immutable once published, so a Cloud pinned to it resolves the same declaration whatever the bundle declares now. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**version** | **int** | The content version number. Numbering starts at 1 and each content patch on the bundle publishes the next one. This is the value a Cloud carries as &#x60;provider_bundle_version&#x60;.  | 
**provider_packages** | [**List[CloudProviderPackage]**](CloudProviderPackage.md) | The Crossplane provider packages this version declares, rendered in canonical source-ascending order.  | 
**provider_config_api_version** | **str** | The &#x60;&lt;group&gt;/&lt;version&gt;&#x60; every package of this version serves its ProviderConfig under (e.g. &#x60;aws.m.upbound.io/v1beta1&#x60;).  | 
**created_by** | **str** | The subject that published the version. Empty for the versions the schema migration backfilled from the bundles that existed before the history was recorded — there is no publishing principal to attribute those to.  | 
**created_at** | **datetime** | Publication timestamp of this version (UTC). | 

## Example

```python
from plexsphere.models.provider_bundle_version import ProviderBundleVersion

# TODO update the JSON string below
json = "{}"
# create an instance of ProviderBundleVersion from a JSON string
provider_bundle_version_instance = ProviderBundleVersion.from_json(json)
# print the JSON string representation of the object
print(ProviderBundleVersion.to_json())

# convert the object into a dict
provider_bundle_version_dict = provider_bundle_version_instance.to_dict()
# create an instance of ProviderBundleVersion from a dict
provider_bundle_version_from_dict = ProviderBundleVersion.from_dict(provider_bundle_version_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


