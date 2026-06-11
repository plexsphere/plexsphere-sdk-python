# PolicyRevisionListResponse

Cursor-paginated page of revisions for a single Policy in descending `created_at` order so the most recent revision leads the page. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**items** | [**List[PolicyRevision]**](PolicyRevision.md) |  | 
**next_cursor** | **str** | Continuation token for the next page. &#x60;null&#x60; or omitted when the iteration has reached end-of-stream.  | [optional] 

## Example

```python
from plexsphere.models.policy_revision_list_response import PolicyRevisionListResponse

# TODO update the JSON string below
json = "{}"
# create an instance of PolicyRevisionListResponse from a JSON string
policy_revision_list_response_instance = PolicyRevisionListResponse.from_json(json)
# print the JSON string representation of the object
print(PolicyRevisionListResponse.to_json())

# convert the object into a dict
policy_revision_list_response_dict = policy_revision_list_response_instance.to_dict()
# create an instance of PolicyRevisionListResponse from a dict
policy_revision_list_response_from_dict = PolicyRevisionListResponse.from_dict(policy_revision_list_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


