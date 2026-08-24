# CloudPatchRequest

Body for `PATCH /v1/clouds/{id}`. All properties are optional — but the body MUST set at least one of `display_name`, `endpoint`, `region_defaults`, `provider_packages`, `provider_config_api_version`, `provider_bundle_id`, `provider_bundle_version`, or `provider_package_overrides`. An empty body is rejected at the handler with `400 empty_patch`. A `provider_packages` patch replaces the whole set as one unit: the packages the body names become the Cloud's packages and every package it omits is dropped. An empty array, a duplicate `source`, or a package missing one of its two members is rejected with `400 invalid_cloud`.  The two provider-configuration modes are exclusive here the way they are on create: a patch names EITHER `provider_bundle_id` OR the inline pair, never both, and one that names both is rejected with `400 invalid_cloud_provider_mode`. Setting `provider_bundle_id` on a Cloud that declares its packages inline is the switch into bundle mode; setting BOTH inline fields on a Cloud that references a bundle is the switch back. A patch that takes a referencing Cloud only halfway out of bundle mode is rejected with `400 invalid_cloud_provider_mode` naming the field it left out, because such a Cloud has neither a package set nor an apiVersion of its own to fall back on.  `provider_bundle_version` belongs to bundle mode as well. Alongside `provider_bundle_id` it pins the version the new reference takes, and omitting it there takes the bundle's latest. On its own, against a Cloud already in bundle mode, it is the promotion: the one write that moves this Cloud onto another declaration of the bundle it already references, and it moves no other Cloud. Naming it while the Cloud is not in bundle mode, and the patch does not put it there, is rejected with `400 invalid_cloud_provider_mode`.  `provider_package_overrides` belongs to bundle mode on the same terms, and is rejected with `400 invalid_cloud_provider_mode` when the patched Cloud ends the write declaring its packages inline. It rides along with an attach or a promotion in the same patch, and the stated set is what the Cloud carries afterwards whichever of the two the patch also did.  The immutable `slug` and `provider` are intentionally absent from this schema; the handler rejects a body carrying `slug` with `400 slug_immutable` and one carrying `provider` with `400 provider_immutable`. `provider` is the validator-routing key for the per-provider validator family — changing it would invalidate every previously-stored endpoint blob. See the `cloud` tag description and the DECISION on `cloud.Cloud`. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**display_name** | **str** | New human-readable Cloud name. The aggregate&#39;s &#x60;Rename&#x60; mutator validates the same constraints as &#x60;NewCloud&#x60;.  | [optional] 
**endpoint** | **Dict[str, object]** | New provider-specific connection metadata. Triggers a re-run of the per-provider validator on the merged next- state.  | [optional] 
**region_defaults** | **Dict[str, object]** | New provider-specific region/default metadata. Triggers a re-run of the per-provider validator on the merged next- state.  | [optional] 
**provider_packages** | [**List[CloudProviderPackage]**](CloudProviderPackage.md) | Replacement package set. The whole set is replaced rather than merged. Omit the field to leave the current set untouched. The read surfaces render the result in canonical source-ascending order.  | [optional] 
**provider_config_api_version** | **str** | New &#x60;&lt;group&gt;/&lt;version&gt;&#x60; every declared package serves its ProviderConfig under. Patchable independently of &#x60;provider_packages&#x60;, except on a Cloud leaving bundle mode, where both inline fields have to be stated together.  | [optional] 
**provider_bundle_id** | **str** | Identifier (UUID) of the provider bundle the Cloud takes its provider configuration from after the patch. Forbidden alongside &#x60;provider_packages&#x60; and &#x60;provider_config_api_version&#x60;. The bundle must exist and must serve the same &#x60;provider&#x60; as the Cloud: an unknown id is rejected with &#x60;400 unknown_provider_bundle&#x60;, a bundle of another provider with &#x60;400 provider_bundle_provider_mismatch&#x60;.  DECISION: the field carries no &#x60;format: uuid&#x60;, for the reason the create request records — a format-annotated field is rejected during JSON decoding, so a malformed id would answer &#x60;400 invalid_body&#x60; before the service&#39;s admission check runs and the operator would never learn which field was wrong.  | [optional] 
**provider_bundle_version** | **int** | The content version of the referenced bundle the Cloud pins after the patch. Omitted alongside a &#x60;provider_bundle_id&#x60; that puts the Cloud into bundle mode, the write pins that bundle&#39;s latest version; on its own it promotes a Cloud already in bundle mode onto the named version.  Naming it while the Cloud neither is nor becomes a bundle reference is rejected with &#x60;400 invalid_cloud_provider_mode&#x60;. A version the bundle never published is rejected with &#x60;400 provider_bundle_version_not_found&#x60;.  | [optional] 
**provider_package_overrides** | [**List[CloudProviderPackage]**](CloudProviderPackage.md) | Replacement override set. The whole set is replaced rather than merged. Omit the field to leave the current set untouched; state &#x60;[]&#x60; to clear it, which puts the Cloud back on the pinned bundle version as it stands. Each &#x60;source&#x60; may appear only once.  Stating it while the patched Cloud ends the write declaring its packages inline is rejected with &#x60;400 invalid_cloud_provider_mode&#x60;.  | [optional] 

## Example

```python
from plexsphere.models.cloud_patch_request import CloudPatchRequest

# TODO update the JSON string below
json = "{}"
# create an instance of CloudPatchRequest from a JSON string
cloud_patch_request_instance = CloudPatchRequest.from_json(json)
# print the JSON string representation of the object
print(CloudPatchRequest.to_json())

# convert the object into a dict
cloud_patch_request_dict = cloud_patch_request_instance.to_dict()
# create an instance of CloudPatchRequest from a dict
cloud_patch_request_from_dict = CloudPatchRequest.from_dict(cloud_patch_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


