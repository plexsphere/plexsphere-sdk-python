# LabelSelectorPreviewResponse

Response for POST /v1/labels/selectors/preview. Either `ast` is populated with the parsed AST or `errors` lists the parse failures — the arrays are mutually exclusive for a single call. HTTP status is always 200 on a well-formed body; the handler never raises on syntax failure. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**ast** | [**LabelSelectorASTNode**](LabelSelectorASTNode.md) | Parsed AST root, when parsing succeeded. | [optional] 
**errors** | [**List[LabelSelectorError]**](LabelSelectorError.md) | Parse errors, when parsing failed. | [optional] 

## Example

```python
from plexsphere.models.label_selector_preview_response import LabelSelectorPreviewResponse

# TODO update the JSON string below
json = "{}"
# create an instance of LabelSelectorPreviewResponse from a JSON string
label_selector_preview_response_instance = LabelSelectorPreviewResponse.from_json(json)
# print the JSON string representation of the object
print(LabelSelectorPreviewResponse.to_json())

# convert the object into a dict
label_selector_preview_response_dict = label_selector_preview_response_instance.to_dict()
# create an instance of LabelSelectorPreviewResponse from a dict
label_selector_preview_response_from_dict = LabelSelectorPreviewResponse.from_dict(label_selector_preview_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


