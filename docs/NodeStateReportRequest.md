# NodeStateReportRequest

Body for `PUT /v1/nodes/{id}/state/reports/{key}`. Carries the upstream, workload-authored report value the addressed Node is recording for the addressed key. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**value** | **str** | The report value to store for the addressed key. Capped at 4096 bytes; an over-size value is rejected with 400.  | 
**workload_tag** | **str** | Optional tag of the workload producing the report. Stored verbatim and echoed back on the next reconciliation pull.  | [optional] 

## Example

```python
from plexsphere.models.node_state_report_request import NodeStateReportRequest

# TODO update the JSON string below
json = "{}"
# create an instance of NodeStateReportRequest from a JSON string
node_state_report_request_instance = NodeStateReportRequest.from_json(json)
# print the JSON string representation of the object
print(NodeStateReportRequest.to_json())

# convert the object into a dict
node_state_report_request_dict = node_state_report_request_instance.to_dict()
# create an instance of NodeStateReportRequest from a dict
node_state_report_request_from_dict = NodeStateReportRequest.from_dict(node_state_report_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


