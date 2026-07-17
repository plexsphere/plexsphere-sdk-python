# PolicyList

Cursor-paginated page of Policies in a Project. Per-row visibility is layered on top of the persistence-level page so `items` is the subset the caller is authorised to see; `next_cursor` always reflects the persistence-level boundary, not the filtered count, so callers cannot infer the size of the hidden set from the cursor. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**items** | [**List[Policy]**](Policy.md) |  | 
**next_cursor** | **str** | Continuation token for the next page. Absent when the iteration has reached end-of-stream.  | [optional] 

## Example

```python
from plexsphere.models.policy_list import PolicyList

# TODO update the JSON string below
json = "{}"
# create an instance of PolicyList from a JSON string
policy_list_instance = PolicyList.from_json(json)
# print the JSON string representation of the object
print(PolicyList.to_json())

# convert the object into a dict
policy_list_dict = policy_list_instance.to_dict()
# create an instance of PolicyList from a dict
policy_list_from_dict = PolicyList.from_dict(policy_list_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


