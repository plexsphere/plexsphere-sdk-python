# GroupMembershipResponse

Response echo of a persisted Membership row . 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**group_id** | **UUID** | Group identifier (UUIDv7). | 
**principal_id** | **UUID** | Principal identifier (UUIDv7). | 
**principal_kind** | **str** | Principal discriminator. | 
**source** | **str** | Provenance of the Membership. | 
**created_at** | **datetime** |  | 

## Example

```python
from plexsphere.models.group_membership_response import GroupMembershipResponse

# TODO update the JSON string below
json = "{}"
# create an instance of GroupMembershipResponse from a JSON string
group_membership_response_instance = GroupMembershipResponse.from_json(json)
# print the JSON string representation of the object
print(GroupMembershipResponse.to_json())

# convert the object into a dict
group_membership_response_dict = group_membership_response_instance.to_dict()
# create an instance of GroupMembershipResponse from a dict
group_membership_response_from_dict = GroupMembershipResponse.from_dict(group_membership_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


