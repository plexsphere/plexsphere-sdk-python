# InvitationList

Page of invitations returned by `GET /v1/domains/{id}/invitations`. The window is computed by the persistence layer in (created_at DESC, id DESC) order; per-row visibility is layered on top so the `items` array is the subset the caller is authorised to see. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**items** | [**List[InvitationResponse]**](InvitationResponse.md) | Invitations in the current page. | 
**next_cursor** | **str** | Continuation token for the next page. &#x60;null&#x60; or omitted when the iteration has reached end-of-stream. The encoding is HMAC-signed by the server so a tampered cursor surfaces as &#x60;400 invalid_cursor&#x60; on the next call.  | [optional] 

## Example

```python
from plexsphere.models.invitation_list import InvitationList

# TODO update the JSON string below
json = "{}"
# create an instance of InvitationList from a JSON string
invitation_list_instance = InvitationList.from_json(json)
# print the JSON string representation of the object
print(InvitationList.to_json())

# convert the object into a dict
invitation_list_dict = invitation_list_instance.to_dict()
# create an instance of InvitationList from a dict
invitation_list_from_dict = InvitationList.from_dict(invitation_list_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


