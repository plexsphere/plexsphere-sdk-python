# CloudProviderPackage

One Crossplane provider package a Cloud declares: which package, at which version. A Cloud names one or more of these in `provider_packages`, and a Cloud in bundle mode authors the same object under `provider_package_overrides` to run one of them differently; the apiVersion every declared package serves its ProviderConfig under is a Cloud-level value, not a per-package one, so it lives on the Cloud rather than here. The two members always travel together, so a partially-filled object is rejected with `400 invalid_cloud`. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**source** | **str** | OCI repository the provider package is pulled from, without a tag or digest (e.g. &#x60;xpkg.upbound.io/upbound/provider-aws-s3&#x60;). A tag belongs in &#x60;version&#x60;.  | 
**version** | **str** | Package version: an OCI tag, optionally pinned with a sha256 digest (e.g. &#x60;v2.6.1&#x60; or &#x60;v2.6.1@sha256:&lt;64 hex&gt;&#x60;). The combined tag-and-digest form is accepted because the Crossplane package manager consumes it verbatim.  | 
**origin** | **str** | Where this member of the effective set came from. Rendered under &#x60;provider_packages&#x60; exactly when the response carries &#x60;provider_bundle_id&#x60;; an inline Cloud&#39;s packages render without it, because a Cloud that owns its declaration has one source for all of it, and the authored entries under &#x60;provider_package_overrides&#x60; render without it because an override is the deviation rather than a member of the set.  &#x60;bundle&#x60; is a member the Cloud takes as the pinned version states it. &#x60;override&#x60; is a member whose version a per-Cloud override replaced. &#x60;addition&#x60; is an override whose source the pinned version does not carry, so the override joined the set rather than shadowing an entry of it.  | [optional] [readonly] 

## Example

```python
from plexsphere.models.cloud_provider_package import CloudProviderPackage

# TODO update the JSON string below
json = "{}"
# create an instance of CloudProviderPackage from a JSON string
cloud_provider_package_instance = CloudProviderPackage.from_json(json)
# print the JSON string representation of the object
print(CloudProviderPackage.to_json())

# convert the object into a dict
cloud_provider_package_dict = cloud_provider_package_instance.to_dict()
# create an instance of CloudProviderPackage from a dict
cloud_provider_package_from_dict = CloudProviderPackage.from_dict(cloud_provider_package_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


