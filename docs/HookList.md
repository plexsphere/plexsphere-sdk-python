# HookList

Page of discovered hooks returned by `ListHooks`. Per-row visibility is layered on the persistence-level page so `items` is the subset the caller is authorised to see. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**items** | [**List[DiscoveredHook]**](DiscoveredHook.md) | Discovered hooks in the current page. | 
**next_cursor** | **str** | Continuation token for the next page. &#x60;null&#x60; or omitted when the iteration has reached end-of-stream. The encoding is HMAC-signed so a tampered cursor surfaces as &#x60;400&#x60; on the next call.  | [optional] 

## Example

```python
from plexsphere.models.hook_list import HookList

# TODO update the JSON string below
json = "{}"
# create an instance of HookList from a JSON string
hook_list_instance = HookList.from_json(json)
# print the JSON string representation of the object
print(HookList.to_json())

# convert the object into a dict
hook_list_dict = hook_list_instance.to_dict()
# create an instance of HookList from a dict
hook_list_from_dict = HookList.from_dict(hook_list_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


