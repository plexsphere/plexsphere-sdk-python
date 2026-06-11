# ApprovalList

Page of Approvals returned by `GET /v1/approvals`. The window is computed by the persistence layer in creation order; per-row visibility is layered on top so the `items` array is the subset the caller is authorised to see. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**items** | [**List[Approval]**](Approval.md) | Approvals in the current page. | 
**next_cursor** | **str** | Continuation token for the next page. &#x60;null&#x60; or omitted when the iteration has reached end-of-stream. The encoding is HMAC-signed by the server so a tampered cursor surfaces as &#x60;400&#x60; on the next call.  | [optional] 

## Example

```python
from plexsphere.models.approval_list import ApprovalList

# TODO update the JSON string below
json = "{}"
# create an instance of ApprovalList from a JSON string
approval_list_instance = ApprovalList.from_json(json)
# print the JSON string representation of the object
print(ApprovalList.to_json())

# convert the object into a dict
approval_list_dict = approval_list_instance.to_dict()
# create an instance of ApprovalList from a dict
approval_list_from_dict = ApprovalList.from_dict(approval_list_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


