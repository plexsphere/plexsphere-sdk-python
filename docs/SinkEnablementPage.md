# SinkEnablementPage

Cursor-paginated page of sink enablements returned by `GET /v1/projects/{id}/sink-enablements`. `next_cursor` is the value to pass back as the `cursor` query parameter on the next call; it is absent when the iteration has reached end-of-stream. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**items** | [**List[SinkEnablement]**](SinkEnablement.md) | The sink enablements on this page. | 
**next_cursor** | **str** | Continuation token for the next page, absent when the page is the last. The encoding is HMAC-signed by the server so a tampered cursor surfaces as &#x60;400&#x60; on the next call.  | [optional] 

## Example

```python
from plexsphere.models.sink_enablement_page import SinkEnablementPage

# TODO update the JSON string below
json = "{}"
# create an instance of SinkEnablementPage from a JSON string
sink_enablement_page_instance = SinkEnablementPage.from_json(json)
# print the JSON string representation of the object
print(SinkEnablementPage.to_json())

# convert the object into a dict
sink_enablement_page_dict = sink_enablement_page_instance.to_dict()
# create an instance of SinkEnablementPage from a dict
sink_enablement_page_from_dict = SinkEnablementPage.from_dict(sink_enablement_page_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


