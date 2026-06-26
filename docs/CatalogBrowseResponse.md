# CatalogBrowseResponse

The Blueprints a catalog source offers, returned by `GET /v1/blueprint-catalogs/{id}/blueprints`. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**items** | [**List[CatalogBlueprintSummary]**](CatalogBlueprintSummary.md) | Blueprints the bundle offers, in slug order. | 

## Example

```python
from plexsphere.models.catalog_browse_response import CatalogBrowseResponse

# TODO update the JSON string below
json = "{}"
# create an instance of CatalogBrowseResponse from a JSON string
catalog_browse_response_instance = CatalogBrowseResponse.from_json(json)
# print the JSON string representation of the object
print(CatalogBrowseResponse.to_json())

# convert the object into a dict
catalog_browse_response_dict = catalog_browse_response_instance.to_dict()
# create an instance of CatalogBrowseResponse from a dict
catalog_browse_response_from_dict = CatalogBrowseResponse.from_dict(catalog_browse_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


