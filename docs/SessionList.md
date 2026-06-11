# SessionList

Page of Sessions returned by `GET /v1/projects/{project_id}/sessions`. The window is computed by the persistence layer in issuance order. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**items** | [**List[Session]**](Session.md) | Sessions in the current page. | 
**next_cursor** | **str** | Continuation token for the next page. &#x60;null&#x60; or omitted when the iteration has reached end-of-stream. The encoding is HMAC-signed by the server so a tampered cursor surfaces as &#x60;400&#x60; on the next call.  | [optional] 

## Example

```python
from plexsphere.models.session_list import SessionList

# TODO update the JSON string below
json = "{}"
# create an instance of SessionList from a JSON string
session_list_instance = SessionList.from_json(json)
# print the JSON string representation of the object
print(SessionList.to_json())

# convert the object into a dict
session_list_dict = session_list_instance.to_dict()
# create an instance of SessionList from a dict
session_list_from_dict = SessionList.from_dict(session_list_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


