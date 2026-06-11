# IntegrityViolationList

Page of integrity-violation rows returned by `ListIntegrityViolations`. Per-row visibility is layered on the persistence-level page so `items` is the subset the caller is authorised to see. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**items** | [**List[IntegrityViolationRow]**](IntegrityViolationRow.md) | Integrity-violation rows in the current page. | 
**next_cursor** | **str** | Continuation token for the next page. &#x60;null&#x60; or omitted when the iteration has reached end-of-stream. The encoding is HMAC-signed so a tampered cursor surfaces as &#x60;400&#x60; on the next call.  | [optional] 

## Example

```python
from plexsphere.models.integrity_violation_list import IntegrityViolationList

# TODO update the JSON string below
json = "{}"
# create an instance of IntegrityViolationList from a JSON string
integrity_violation_list_instance = IntegrityViolationList.from_json(json)
# print the JSON string representation of the object
print(IntegrityViolationList.to_json())

# convert the object into a dict
integrity_violation_list_dict = integrity_violation_list_instance.to_dict()
# create an instance of IntegrityViolationList from a dict
integrity_violation_list_from_dict = IntegrityViolationList.from_dict(integrity_violation_list_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


