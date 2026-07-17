# LabelDefinitionList

Page of Label Definitions returned by GET /v1/label-definitions. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**items** | [**List[LabelDefinition]**](LabelDefinition.md) |  | 
**next_cursor** | **str** | Continuation token for the next page. Omitted or empty when the iteration has reached end-of-stream.  | [optional] 

## Example

```python
from plexsphere.models.label_definition_list import LabelDefinitionList

# TODO update the JSON string below
json = "{}"
# create an instance of LabelDefinitionList from a JSON string
label_definition_list_instance = LabelDefinitionList.from_json(json)
# print the JSON string representation of the object
print(LabelDefinitionList.to_json())

# convert the object into a dict
label_definition_list_dict = label_definition_list_instance.to_dict()
# create an instance of LabelDefinitionList from a dict
label_definition_list_from_dict = LabelDefinitionList.from_dict(label_definition_list_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


