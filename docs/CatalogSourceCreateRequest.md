# CatalogSourceCreateRequest

Body for `POST /v1/blueprint-catalogs`. The field set mirrors the CatalogSource aggregate's invariants. The handler authorises the call against the source's scope parent (`platform#manage` for a catalog-global source, `domain#manage` for a Domain-scoped one) BEFORE the service persists anything. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**name** | **str** | Human-facing label. Not required unique — identity is the generated &#x60;id&#x60;. Whitespace-only is rejected.  | 
**oci_reference** | [**OciReference**](OciReference.md) |  | 
**verification** | [**VerificationPolicy**](VerificationPolicy.md) |  | 
**tracking** | [**TrackingPolicy**](TrackingPolicy.md) |  | 
**credential_ref** | **str** | Optional &#x60;namespace/name&#x60; reference to the Kubernetes Secret the registry authenticates with. Omit or &#x60;null&#x60; for a public registry. A malformed reference surfaces as &#x60;400 invalid_credential_ref&#x60;.  | [optional] 
**domain_id** | **UUID** | Owning Domain when the source is scoped to a single Domain. Omit or &#x60;null&#x60; for a catalog-global source. The scope drives the ReBAC gate: a Domain-scoped source gates on &#x60;domain#manage&#x60;, a catalog-global source on &#x60;platform#manage&#x60;.  | [optional] 
**status** | **str** | Lifecycle status. &#x60;active&#x60; sources are consulted on import; &#x60;disabled&#x60; keeps the registration on record without consulting it.  | 

## Example

```python
from plexsphere.models.catalog_source_create_request import CatalogSourceCreateRequest

# TODO update the JSON string below
json = "{}"
# create an instance of CatalogSourceCreateRequest from a JSON string
catalog_source_create_request_instance = CatalogSourceCreateRequest.from_json(json)
# print the JSON string representation of the object
print(CatalogSourceCreateRequest.to_json())

# convert the object into a dict
catalog_source_create_request_dict = catalog_source_create_request_instance.to_dict()
# create an instance of CatalogSourceCreateRequest from a dict
catalog_source_create_request_from_dict = CatalogSourceCreateRequest.from_dict(catalog_source_create_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


