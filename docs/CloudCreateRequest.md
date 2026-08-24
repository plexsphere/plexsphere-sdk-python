# CloudCreateRequest

Body for `POST /v1/clouds`. Field set mirrors the Cloud aggregate's `NewCloud` invariants. The handler authorises the call against the platform-level `manage` relation BEFORE invoking the service so an unauthorised caller never produces a `CloudCreated` outbox row.  The Cloud holds its provider configuration in exactly one of two ways, so a request names EITHER `provider_bundle_id` OR the inline pair `provider_packages` + `provider_config_api_version`, never both. A request naming a bundle alongside either inline field is rejected with `400 invalid_cloud_provider_mode` naming every field that took part in the conflict; a request that sets neither, or only one half of the inline pair, is rejected with `400 invalid_cloud`.  A request in bundle mode may name `provider_bundle_version` to pin an older declaration; omitting it pins the bundle's latest. It may also name `provider_package_overrides` to run some of that declaration's packages at other versions. Both fields belong to bundle mode: stating either without `provider_bundle_id` is rejected with `400 invalid_cloud_provider_mode` naming the field. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**display_name** | **str** | Human-readable Cloud name. Whitespace-only is rejected. | 
**slug** | **str** | Kebab-case URL handle. The aggregate&#39;s &#x60;ParseSlug&#x60; enforces the same regex; surfacing the pattern here lets the generated client validate before the round-trip.  | 
**provider** | [**CloudProvider**](CloudProvider.md) |  | 
**endpoint** | **Dict[str, object]** | Provider-specific connection metadata. The per-provider validator runs at decode time; field-level rejections surface as &#x60;400 invalid_cloud_endpoint&#x60;.  | 
**region_defaults** | **Dict[str, object]** | Provider-specific region/default metadata. The per- provider validator runs at decode time; field-level rejections surface as &#x60;400 invalid_cloud_region_defaults&#x60;.  | 
**external_id** | **str** | Upstream provider account identifier. Combined with &#x60;provider&#x60; must be unique across all Clouds.  | 
**provider_packages** | [**List[CloudProviderPackage]**](CloudProviderPackage.md) | The Crossplane provider packages the Cloud declares inline. Required in inline mode and forbidden alongside &#x60;provider_bundle_id&#x60;. Each &#x60;source&#x60; may appear only once. The read surfaces render the set in canonical source-ascending order, not in the order stated here.  | [optional] 
**provider_config_api_version** | **str** | The &#x60;&lt;group&gt;/&lt;version&gt;&#x60; every inline-declared package serves its ProviderConfig under (e.g. &#x60;aws.m.upbound.io/v1beta1&#x60;). Required in inline mode and forbidden alongside &#x60;provider_bundle_id&#x60;. The Provisioning Broker stamps this value as the rendered ProviderConfig&#39;s apiVersion.  | [optional] 
**provider_bundle_id** | **str** | Identifier (UUID) of the provider bundle this Cloud takes its provider configuration from. Naming it puts the Cloud in bundle mode, which forbids &#x60;provider_packages&#x60; and &#x60;provider_config_api_version&#x60; in the same request. The bundle must exist and must serve the same &#x60;provider&#x60; as the Cloud: an unknown id is rejected with &#x60;400 unknown_provider_bundle&#x60;, a bundle of another provider with &#x60;400 provider_bundle_provider_mismatch&#x60;.  DECISION: the field carries no &#x60;format: uuid&#x60;. A format-annotated field is rejected during JSON decoding, so a malformed id would answer &#x60;400 invalid_body&#x60; before the service&#39;s admission check runs and the operator would never learn which field was wrong. The value is a canonical UUID string and a malformed one surfaces as &#x60;400 invalid_cloud_provider_mode&#x60; naming &#x60;provider_bundle_id&#x60;.  | [optional] 
**provider_bundle_version** | **int** | The content version of the named bundle the new Cloud pins. Omit it and the write pins the bundle&#39;s latest version, which is the declaration an operator reads when they pick the bundle; name one to pin an older declaration instead.  The field belongs to bundle mode: setting it without &#x60;provider_bundle_id&#x60; is rejected with &#x60;400 invalid_cloud_provider_mode&#x60;. A version the bundle never published is rejected with &#x60;400 provider_bundle_version_not_found&#x60;.  | [optional] 
**provider_package_overrides** | [**List[CloudProviderPackage]**](CloudProviderPackage.md) | Packages the new Cloud runs differently from the version it pins. Each &#x60;source&#x60; may appear only once. Omit the field, or state the empty array, and the Cloud takes the pinned version as it stands.  The field belongs to bundle mode: setting it without &#x60;provider_bundle_id&#x60; is rejected with &#x60;400 invalid_cloud_provider_mode&#x60;. An override is a deviation from a referenced declaration, and a Cloud that owns its package set runs another version by editing that set.  | [optional] 

## Example

```python
from plexsphere.models.cloud_create_request import CloudCreateRequest

# TODO update the JSON string below
json = "{}"
# create an instance of CloudCreateRequest from a JSON string
cloud_create_request_instance = CloudCreateRequest.from_json(json)
# print the JSON string representation of the object
print(CloudCreateRequest.to_json())

# convert the object into a dict
cloud_create_request_dict = cloud_create_request_instance.to_dict()
# create an instance of CloudCreateRequest from a dict
cloud_create_request_from_dict = CloudCreateRequest.from_dict(cloud_create_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


