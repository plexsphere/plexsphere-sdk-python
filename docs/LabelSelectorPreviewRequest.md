# LabelSelectorPreviewRequest

Body for POST /v1/labels/selectors/preview. Carries the raw selector expression to parse. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**expression** | **str** | Raw label-selector expression (for example &#x60;env&#x3D;prod, owner in (alice, bob)&#x60;).  | 

## Example

```python
from plexsphere.models.label_selector_preview_request import LabelSelectorPreviewRequest

# TODO update the JSON string below
json = "{}"
# create an instance of LabelSelectorPreviewRequest from a JSON string
label_selector_preview_request_instance = LabelSelectorPreviewRequest.from_json(json)
# print the JSON string representation of the object
print(LabelSelectorPreviewRequest.to_json())

# convert the object into a dict
label_selector_preview_request_dict = label_selector_preview_request_instance.to_dict()
# create an instance of LabelSelectorPreviewRequest from a dict
label_selector_preview_request_from_dict = LabelSelectorPreviewRequest.from_dict(label_selector_preview_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


