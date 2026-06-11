# GroupMembershipListResponse

List of Memberships attached to a Group. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**items** | [**List[GroupMembershipResponse]**](GroupMembershipResponse.md) | Memberships of the Group. | 

## Example

```python
from plexsphere.models.group_membership_list_response import GroupMembershipListResponse

# TODO update the JSON string below
json = "{}"
# create an instance of GroupMembershipListResponse from a JSON string
group_membership_list_response_instance = GroupMembershipListResponse.from_json(json)
# print the JSON string representation of the object
print(GroupMembershipListResponse.to_json())

# convert the object into a dict
group_membership_list_response_dict = group_membership_list_response_instance.to_dict()
# create an instance of GroupMembershipListResponse from a dict
group_membership_list_response_from_dict = GroupMembershipListResponse.from_dict(group_membership_list_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


