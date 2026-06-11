# BlueprintResponse

Hydrated projection of a Blueprint Catalog entry. The shape is shared by `ListBlueprints` and `GetBlueprint` — `ListBlueprints` returns it with an empty `versions` array, `GetBlueprint` returns it with the Blueprint's ordered versions. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**id** | **UUID** | Blueprint identifier (UUIDv7). | 
**slug** | **str** | Kebab-case URL handle, unique across the catalogue. | 
**domain_id** | **UUID** | Owning Domain when the Blueprint is scoped to a single Domain. &#x60;null&#x60; when the entry is catalogue-wide.  | [optional] 
**display_name** | **str** | Human-readable Blueprint name. | 
**description** | **str** | Optional free-form Blueprint description. Empty when the catalogue entry declares none.  | [optional] 
**status** | **str** | Lifecycle status. &#x60;active&#x60; entries are offerable; &#x60;retired&#x60; entries remain readable but are no longer offered for new Resources.  | 
**created_at** | **datetime** | Blueprint creation timestamp (UTC). | 
**updated_at** | **datetime** | Last-modified timestamp (UTC). | 
**versions** | [**List[BlueprintVersionResponse]**](BlueprintVersionResponse.md) | Ordered Blueprint versions. Empty on the &#x60;ListBlueprints&#x60; projection; populated on &#x60;GetBlueprint&#x60;.  | 

## Example

```python
from plexsphere.models.blueprint_response import BlueprintResponse

# TODO update the JSON string below
json = "{}"
# create an instance of BlueprintResponse from a JSON string
blueprint_response_instance = BlueprintResponse.from_json(json)
# print the JSON string representation of the object
print(BlueprintResponse.to_json())

# convert the object into a dict
blueprint_response_dict = blueprint_response_instance.to_dict()
# create an instance of BlueprintResponse from a dict
blueprint_response_from_dict = BlueprintResponse.from_dict(blueprint_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


