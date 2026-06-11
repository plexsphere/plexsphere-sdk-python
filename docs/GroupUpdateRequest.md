# GroupUpdateRequest

Body for PATCH /v1/admin/groups/{id}. Only `display_name` is settable; the `slug` is immutable because it participates in the `(domain_id, slug)` uniqueness constraint and is referenced by IdP claim mappings. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**display_name** | **str** | New human-friendly Group name. | 

## Example

```python
from plexsphere.models.group_update_request import GroupUpdateRequest

# TODO update the JSON string below
json = "{}"
# create an instance of GroupUpdateRequest from a JSON string
group_update_request_instance = GroupUpdateRequest.from_json(json)
# print the JSON string representation of the object
print(GroupUpdateRequest.to_json())

# convert the object into a dict
group_update_request_dict = group_update_request_instance.to_dict()
# create an instance of GroupUpdateRequest from a dict
group_update_request_from_dict = GroupUpdateRequest.from_dict(group_update_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


