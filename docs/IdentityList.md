# IdentityList

Page of identities returned by `GET /v1/domains/{id}/identities`. The window is computed by the persistence layer in (created_at DESC, id DESC) order; per-row visibility is layered on top so the `items` array is the subset the caller is authorised to see. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**items** | [**List[IdentitySummary]**](IdentitySummary.md) | Identities in the current page. | 
**next_cursor** | **str** | Continuation token for the next page. &#x60;null&#x60; or omitted when the iteration has reached end-of-stream. The encoding is HMAC-signed by the server so a tampered cursor surfaces as &#x60;400 invalid_cursor&#x60; on the next call.  | [optional] 

## Example

```python
from plexsphere.models.identity_list import IdentityList

# TODO update the JSON string below
json = "{}"
# create an instance of IdentityList from a JSON string
identity_list_instance = IdentityList.from_json(json)
# print the JSON string representation of the object
print(IdentityList.to_json())

# convert the object into a dict
identity_list_dict = identity_list_instance.to_dict()
# create an instance of IdentityList from a dict
identity_list_from_dict = IdentityList.from_dict(identity_list_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


