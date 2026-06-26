# CatalogSourceResponse

Hydrated projection of a registered catalog source. The shape is shared by `RegisterCatalogSource`, `ListCatalogSources`, and `GetCatalogSource`. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**id** | **UUID** | Catalog source identifier (UUIDv7). | 
**name** | **str** | Human-facing label. | 
**oci_reference** | [**OciReference**](OciReference.md) |  | 
**verification** | [**VerificationPolicy**](VerificationPolicy.md) |  | 
**tracking** | [**TrackingPolicy**](TrackingPolicy.md) |  | 
**credential_ref** | **str** | &#x60;namespace/name&#x60; reference to the registry-credential Secret, or &#x60;null&#x60; when the source needs none.  | [optional] 
**domain_id** | **UUID** | Owning Domain, or &#x60;null&#x60; for a catalog-global source.  | [optional] 
**last_resolved_digest** | **str** | The bundle digest the source last resolved to, or &#x60;null&#x60; before the first resolution.  | [optional] 
**status** | **str** | Lifecycle status. | 
**created_at** | **datetime** | Catalog source creation timestamp (UTC). | 
**updated_at** | **datetime** | Last-modified timestamp (UTC). | 

## Example

```python
from plexsphere.models.catalog_source_response import CatalogSourceResponse

# TODO update the JSON string below
json = "{}"
# create an instance of CatalogSourceResponse from a JSON string
catalog_source_response_instance = CatalogSourceResponse.from_json(json)
# print the JSON string representation of the object
print(CatalogSourceResponse.to_json())

# convert the object into a dict
catalog_source_response_dict = catalog_source_response_instance.to_dict()
# create an instance of CatalogSourceResponse from a dict
catalog_source_response_from_dict = CatalogSourceResponse.from_dict(catalog_source_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


