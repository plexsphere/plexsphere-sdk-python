# CatalogSourceList

The registered catalog sources returned by `GET /v1/blueprint-catalogs`. The set is small, operator-curated configuration returned whole; per-row visibility is layered on top so `items` is the subset the caller is authorised to see. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**items** | [**List[CatalogSourceResponse]**](CatalogSourceResponse.md) | Registered catalog sources. | 

## Example

```python
from plexsphere.models.catalog_source_list import CatalogSourceList

# TODO update the JSON string below
json = "{}"
# create an instance of CatalogSourceList from a JSON string
catalog_source_list_instance = CatalogSourceList.from_json(json)
# print the JSON string representation of the object
print(CatalogSourceList.to_json())

# convert the object into a dict
catalog_source_list_dict = catalog_source_list_instance.to_dict()
# create an instance of CatalogSourceList from a dict
catalog_source_list_from_dict = CatalogSourceList.from_dict(catalog_source_list_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


