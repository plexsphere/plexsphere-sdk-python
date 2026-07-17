# GroupMembershipList

List of Memberships attached to a Group. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**items** | [**List[GroupMembershipResponse]**](GroupMembershipResponse.md) | Memberships of the Group. | 

## Example

```python
from plexsphere.models.group_membership_list import GroupMembershipList

# TODO update the JSON string below
json = "{}"
# create an instance of GroupMembershipList from a JSON string
group_membership_list_instance = GroupMembershipList.from_json(json)
# print the JSON string representation of the object
print(GroupMembershipList.to_json())

# convert the object into a dict
group_membership_list_dict = group_membership_list_instance.to_dict()
# create an instance of GroupMembershipList from a dict
group_membership_list_from_dict = GroupMembershipList.from_dict(group_membership_list_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


