# LabelSelectorError

Parse-time error report carrying the byte offset and a human-readable message so editor integrations can render inline squiggles. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**position** | **int** | Byte offset of the offending token. | 
**message** | **str** | Human-readable error detail. | 

## Example

```python
from plexsphere.models.label_selector_error import LabelSelectorError

# TODO update the JSON string below
json = "{}"
# create an instance of LabelSelectorError from a JSON string
label_selector_error_instance = LabelSelectorError.from_json(json)
# print the JSON string representation of the object
print(LabelSelectorError.to_json())

# convert the object into a dict
label_selector_error_dict = label_selector_error_instance.to_dict()
# create an instance of LabelSelectorError from a dict
label_selector_error_from_dict = LabelSelectorError.from_dict(label_selector_error_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


