# PolicyListResponse

Cursor-paginated page of Policies in a Project. Per-row visibility is layered on top of the persistence-level page so `items` is the subset the caller is authorised to see; `next_cursor` always reflects the persistence-level boundary, not the filtered count, so callers cannot infer the size of the hidden set from the cursor. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**items** | [**List[Policy]**](Policy.md) |  | 
**next_cursor** | **str** | Continuation token for the next page. &#x60;null&#x60; or omitted when the iteration has reached end-of-stream.  | [optional] 

## Example

```python
from plexsphere.models.policy_list_response import PolicyListResponse

# TODO update the JSON string below
json = "{}"
# create an instance of PolicyListResponse from a JSON string
policy_list_response_instance = PolicyListResponse.from_json(json)
# print the JSON string representation of the object
print(PolicyListResponse.to_json())

# convert the object into a dict
policy_list_response_dict = policy_list_response_instance.to_dict()
# create an instance of PolicyListResponse from a dict
policy_list_response_from_dict = PolicyListResponse.from_dict(policy_list_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


