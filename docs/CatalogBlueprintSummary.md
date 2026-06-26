# CatalogBlueprintSummary

One Blueprint a catalog source's bundle offers, as returned by `BrowseCatalogSource`. It carries the bundle metadata only — the Blueprint is not imported until `ImportFromCatalogSource` runs. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**slug** | **str** | Kebab-case URL handle the Blueprint imports under. | 
**display_name** | **str** | Human-readable Blueprint name from the bundle metadata. | 
**description** | **str** | Optional free-form description from the bundle metadata. | [optional] 
**version** | **str** | The version label the bundle offers for this Blueprint. | 
**provider_kinds** | **List[str]** | Infrastructure substrates the offered version targets, as declared in the bundle metadata.  | 
**injection_strategy** | **str** | How the offered version threads request parameters into the rendered Composite Resource, as declared in the bundle metadata.  | 

## Example

```python
from plexsphere.models.catalog_blueprint_summary import CatalogBlueprintSummary

# TODO update the JSON string below
json = "{}"
# create an instance of CatalogBlueprintSummary from a JSON string
catalog_blueprint_summary_instance = CatalogBlueprintSummary.from_json(json)
# print the JSON string representation of the object
print(CatalogBlueprintSummary.to_json())

# convert the object into a dict
catalog_blueprint_summary_dict = catalog_blueprint_summary_instance.to_dict()
# create an instance of CatalogBlueprintSummary from a dict
catalog_blueprint_summary_from_dict = CatalogBlueprintSummary.from_dict(catalog_blueprint_summary_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


