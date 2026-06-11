# NodeStateReportResponse

Result of an accepted `PUT /v1/nodes/{id}/state/reports/{key}`. Echoes the stored key and the server wall-clock time at which the report write was accepted. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**accepted_at** | **datetime** | Server wall-clock time at which the report write was accepted and durably recorded.  | 
**key** | **str** | The report key that was written. | 

## Example

```python
from plexsphere.models.node_state_report_response import NodeStateReportResponse

# TODO update the JSON string below
json = "{}"
# create an instance of NodeStateReportResponse from a JSON string
node_state_report_response_instance = NodeStateReportResponse.from_json(json)
# print the JSON string representation of the object
print(NodeStateReportResponse.to_json())

# convert the object into a dict
node_state_report_response_dict = node_state_report_response_instance.to_dict()
# create an instance of NodeStateReportResponse from a dict
node_state_report_response_from_dict = NodeStateReportResponse.from_dict(node_state_report_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


