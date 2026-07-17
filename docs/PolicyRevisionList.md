# PolicyRevisionList

Cursor-paginated page of revisions for a single Policy in descending `created_at` order so the most recent revision leads the page. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**items** | [**List[PolicyRevision]**](PolicyRevision.md) |  | 
**next_cursor** | **str** | Continuation token for the next page. Absent when the iteration has reached end-of-stream.  | [optional] 

## Example

```python
from plexsphere.models.policy_revision_list import PolicyRevisionList

# TODO update the JSON string below
json = "{}"
# create an instance of PolicyRevisionList from a JSON string
policy_revision_list_instance = PolicyRevisionList.from_json(json)
# print the JSON string representation of the object
print(PolicyRevisionList.to_json())

# convert the object into a dict
policy_revision_list_dict = policy_revision_list_instance.to_dict()
# create an instance of PolicyRevisionList from a dict
policy_revision_list_from_dict = PolicyRevisionList.from_dict(policy_revision_list_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


