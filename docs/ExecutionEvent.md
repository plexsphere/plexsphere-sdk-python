# ExecutionEvent

Append-only event row recording one status advance in an Execution's history. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**id** | **UUID** | Event identifier (UUIDv7). | 
**status** | [**ExecutionStatus**](ExecutionStatus.md) |  | 
**occurred_at** | **datetime** | Timestamp the status advance occurred (UTC). | 
**detail** | **str** | Optional free-text detail recorded with the advance — for a failed status this carries the reported error text.  | [optional] 

## Example

```python
from plexsphere.models.execution_event import ExecutionEvent

# TODO update the JSON string below
json = "{}"
# create an instance of ExecutionEvent from a JSON string
execution_event_instance = ExecutionEvent.from_json(json)
# print the JSON string representation of the object
print(ExecutionEvent.to_json())

# convert the object into a dict
execution_event_dict = execution_event_instance.to_dict()
# create an instance of ExecutionEvent from a dict
execution_event_from_dict = ExecutionEvent.from_dict(execution_event_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


