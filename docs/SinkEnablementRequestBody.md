# SinkEnablementRequestBody

Body for `POST /v1/projects/{id}/sink-enablements`. Names the sink the Project asks to be enabled on. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**sink_id** | **UUID** | Identifier of the sink to enable. Must be a non-zero UUID naming a tenant sink of the Project&#39;s own Domain.  | 

## Example

```python
from plexsphere.models.sink_enablement_request_body import SinkEnablementRequestBody

# TODO update the JSON string below
json = "{}"
# create an instance of SinkEnablementRequestBody from a JSON string
sink_enablement_request_body_instance = SinkEnablementRequestBody.from_json(json)
# print the JSON string representation of the object
print(SinkEnablementRequestBody.to_json())

# convert the object into a dict
sink_enablement_request_body_dict = sink_enablement_request_body_instance.to_dict()
# create an instance of SinkEnablementRequestBody from a dict
sink_enablement_request_body_from_dict = SinkEnablementRequestBody.from_dict(sink_enablement_request_body_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


