# CloudResponse

Hydrated projection of a Cloud aggregate. The shape is shared by `CreateCloud`, `GetCloud`, `ListClouds`, and `PatchCloud` — every read surface returns the same wire bytes for the same Cloud so clients only need one binding. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**id** | **UUID** | Cloud identifier (UUIDv7). | 
**display_name** | **str** | Human-readable Cloud name (trimmed at the aggregate). | 
**slug** | **str** | Kebab-case URL handle. Stable for the lifetime of the Cloud — see the &#x60;cloud&#x60; tag description for the immutability rationale.  | 
**provider** | [**CloudProvider**](CloudProvider.md) |  | 
**endpoint** | **Dict[str, object]** | Provider-specific connection metadata stored as a JSONB blob. The per-provider validator owns the field-shape contract; this schema only declares the wire envelope.  | 
**region_defaults** | **Dict[str, object]** | Provider-specific region/default metadata stored as a JSONB blob. The per-provider validator owns the field- shape contract.  | 
**external_id** | **str** | Upstream provider account identifier (e.g. AWS account id, Azure tenant id). Combined with &#x60;provider&#x60; it must be unique across all Clouds.  | 
**provider_packages** | [**List[CloudProviderPackage]**](CloudProviderPackage.md) | The EFFECTIVE Crossplane provider packages for this Cloud: the pinned provider bundle version&#39;s set with the Cloud&#39;s &#x60;provider_package_overrides&#x60; merged over it when &#x60;provider_bundle_id&#x60; is present, otherwise the set the Cloud declares inline. Rendered in canonical source-ascending order regardless of the order the operator stated them in. A &#x60;PATCH /v1/clouds/{id}&#x60; carrying &#x60;provider_packages&#x60; replaces the whole inline set.  | 
**provider_config_api_version** | **str** | The EFFECTIVE &#x60;&lt;group&gt;/&lt;version&gt;&#x60; every declared package serves its ProviderConfig under (e.g. &#x60;aws.m.upbound.io/v1beta1&#x60;): the referenced provider bundle&#39;s value when &#x60;provider_bundle_id&#x60; is present, otherwise the Cloud&#39;s own. The Provisioning Broker stamps this value as the rendered ProviderConfig&#39;s apiVersion.  | 
**provider_bundle_id** | **UUID** | Identifier of the provider bundle this Cloud takes its provider configuration from. Present only for a Cloud in bundle mode; a Cloud that declares its packages inline omits the field.  | [optional] 
**provider_bundle_slug** | **str** | Kebab-case handle of the referenced provider bundle, carried alongside the id so a client renders the reference without a second round-trip. Present only for a Cloud in bundle mode.  | [optional] 
**provider_bundle_version** | **int** | The content version of that bundle the Cloud pins — the declaration &#x60;provider_packages&#x60; and &#x60;provider_config_api_version&#x60; above were resolved from. Present exactly when &#x60;provider_bundle_id&#x60; is; a Cloud that declares its packages inline omits all three fields. It moves only on a Cloud write, so a bundle patch that publishes a newer version leaves this value alone.  | [optional] 
**provider_package_overrides** | [**List[CloudProviderPackage]**](CloudProviderPackage.md) | The packages this Cloud runs differently from the version it pins, as the operator authored them, in canonical source-ascending order. Present exactly when &#x60;provider_bundle_id&#x60; is, and an empty array for a Cloud that states none; a Cloud that declares its packages inline omits the field.  An override whose source the pinned version carries replaces that member&#39;s version, and the merged entry reports &#x60;origin: override&#x60; under &#x60;provider_packages&#x60;. An override naming a source the pinned version does not carry joins the effective set and reports &#x60;origin: addition&#x60;. Nothing is ever removed, so the effective set is never smaller than the pinned version&#39;s.  | [optional] 
**created_at** | **datetime** | Aggregate creation timestamp (UTC). | 
**updated_at** | **datetime** | Last-modified timestamp (UTC). Bumped by every mutator — &#x60;Rename&#x60;, &#x60;ChangeEndpoint&#x60;, &#x60;ChangeRegionDefaults&#x60;, &#x60;ChangeProviderPackages&#x60;, &#x60;ChangeProviderConfigAPIVersion&#x60;, &#x60;ReferenceProviderBundle&#x60;, &#x60;DeclareInlinePackages&#x60;.  | 

## Example

```python
from plexsphere.models.cloud_response import CloudResponse

# TODO update the JSON string below
json = "{}"
# create an instance of CloudResponse from a JSON string
cloud_response_instance = CloudResponse.from_json(json)
# print the JSON string representation of the object
print(CloudResponse.to_json())

# convert the object into a dict
cloud_response_dict = cloud_response_instance.to_dict()
# create an instance of CloudResponse from a dict
cloud_response_from_dict = CloudResponse.from_dict(cloud_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


