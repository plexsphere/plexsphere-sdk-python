# CapabilityRowList

Page of capability rows returned by `ListCapabilities`. Per-row visibility is layered on the persistence-level page so `items` is the subset the caller is authorised to see. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**items** | [**List[CapabilityRow]**](CapabilityRow.md) | Capability rows in the current page. | 
**next_cursor** | **str** | Continuation token for the next page. &#x60;null&#x60; or omitted when the iteration has reached end-of-stream. The encoding is HMAC-signed so a tampered cursor surfaces as &#x60;400&#x60; on the next call.  | [optional] 

## Example

```python
from plexsphere.models.capability_row_list import CapabilityRowList

# TODO update the JSON string below
json = "{}"
# create an instance of CapabilityRowList from a JSON string
capability_row_list_instance = CapabilityRowList.from_json(json)
# print the JSON string representation of the object
print(CapabilityRowList.to_json())

# convert the object into a dict
capability_row_list_dict = capability_row_list_instance.to_dict()
# create an instance of CapabilityRowList from a dict
capability_row_list_from_dict = CapabilityRowList.from_dict(capability_row_list_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


