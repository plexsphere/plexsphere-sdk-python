# LabelDefinitionUpdateRequest

Body for PATCH /v1/label-definitions/{id}. Only mutable fields may be changed. `scope`, `scope_id`, `local_key`, and `immutable` are frozen after creation. Immutable Definitions reject `value_schema` changes with 409 `immutable-violation` . 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**description** | **str** |  | [optional] 
**value_schema** | [**LabelValueSchema**](LabelValueSchema.md) |  | [optional] 
**applicable_kinds** | **List[str]** |  | [optional] 
**required** | **bool** |  | [optional] 
**default_value** | **object** |  | [optional] 
**cloud_tag_propagation** | **bool** |  | [optional] 
**on_delete** | **str** |  | [optional] 

## Example

```python
from plexsphere.models.label_definition_update_request import LabelDefinitionUpdateRequest

# TODO update the JSON string below
json = "{}"
# create an instance of LabelDefinitionUpdateRequest from a JSON string
label_definition_update_request_instance = LabelDefinitionUpdateRequest.from_json(json)
# print the JSON string representation of the object
print(LabelDefinitionUpdateRequest.to_json())

# convert the object into a dict
label_definition_update_request_dict = label_definition_update_request_instance.to_dict()
# create an instance of LabelDefinitionUpdateRequest from a dict
label_definition_update_request_from_dict = LabelDefinitionUpdateRequest.from_dict(label_definition_update_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


