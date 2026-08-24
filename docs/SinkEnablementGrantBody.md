# SinkEnablementGrantBody

Body for `POST /v1/sinks/{id}/sink-enablements`. Names the Project the sink's authority grants it to. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**project_id** | **UUID** | Identifier of the Project to enable the sink in. Must be a non-zero UUID naming a Project of the sink&#39;s own Domain.  | 

## Example

```python
from plexsphere.models.sink_enablement_grant_body import SinkEnablementGrantBody

# TODO update the JSON string below
json = "{}"
# create an instance of SinkEnablementGrantBody from a JSON string
sink_enablement_grant_body_instance = SinkEnablementGrantBody.from_json(json)
# print the JSON string representation of the object
print(SinkEnablementGrantBody.to_json())

# convert the object into a dict
sink_enablement_grant_body_dict = sink_enablement_grant_body_instance.to_dict()
# create an instance of SinkEnablementGrantBody from a dict
sink_enablement_grant_body_from_dict = SinkEnablementGrantBody.from_dict(sink_enablement_grant_body_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


