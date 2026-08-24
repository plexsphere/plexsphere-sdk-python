# ProjectBlueprintOffer

A Blueprint Catalog entry seen from inside one Project: the catalogue metadata, the provider kinds its published versions accept, and whether the Project can provision it today. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**id** | **UUID** | Blueprint identifier (UUIDv7). | 
**slug** | **str** | Kebab-case URL handle, unique across the catalogue. | 
**display_name** | **str** | Human-readable Blueprint name. | 
**description** | **str** | Optional free-form Blueprint description. Absent when the catalogue entry declares none.  | [optional] 
**status** | **str** | Lifecycle status. &#x60;active&#x60; entries are offerable; &#x60;retired&#x60; entries remain readable but are no longer offered for new Resources.  | 
**provider_kinds** | **List[str]** | Union of the substrates the Blueprint&#39;s published versions accept, deduplicated and sorted ascending. Empty when the Blueprint has no published version, which also makes &#x60;provisionable&#x60; false.  | 
**provisionable** | **bool** | True when &#x60;provider_kinds&#x60; intersects the Project&#39;s &#x60;reachable_provider_kinds&#x60;. The verdict is assignment-level: it does not promise an assigned credential for the matching Cloud.  | 
**created_at** | **datetime** | Blueprint creation timestamp (UTC). | 
**updated_at** | **datetime** | Last-modified timestamp (UTC). | 

## Example

```python
from plexsphere.models.project_blueprint_offer import ProjectBlueprintOffer

# TODO update the JSON string below
json = "{}"
# create an instance of ProjectBlueprintOffer from a JSON string
project_blueprint_offer_instance = ProjectBlueprintOffer.from_json(json)
# print the JSON string representation of the object
print(ProjectBlueprintOffer.to_json())

# convert the object into a dict
project_blueprint_offer_dict = project_blueprint_offer_instance.to_dict()
# create an instance of ProjectBlueprintOffer from a dict
project_blueprint_offer_from_dict = ProjectBlueprintOffer.from_dict(project_blueprint_offer_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


