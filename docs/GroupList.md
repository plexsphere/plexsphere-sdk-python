# GroupList

Page of Groups returned by GET /v1/admin/groups. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**items** | [**List[GroupResponse]**](GroupResponse.md) | Groups in the current page. | 
**next_cursor** | **str** | Continuation token for the next page. Omitted or empty when the iteration has reached end-of-stream.  | [optional] 

## Example

```python
from plexsphere.models.group_list import GroupList

# TODO update the JSON string below
json = "{}"
# create an instance of GroupList from a JSON string
group_list_instance = GroupList.from_json(json)
# print the JSON string representation of the object
print(GroupList.to_json())

# convert the object into a dict
group_list_dict = group_list_instance.to_dict()
# create an instance of GroupList from a dict
group_list_from_dict = GroupList.from_dict(group_list_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


