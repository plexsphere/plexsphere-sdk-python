# ExecutionList

Page of Executions returned by `GET /v1/projects/{project_id}/executions`. The window is computed by the persistence layer in creation order. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**items** | [**List[Execution]**](Execution.md) | Executions in the current page. | 
**next_cursor** | **str** | Continuation token for the next page. &#x60;null&#x60; or omitted when the iteration has reached end-of-stream. The encoding is HMAC-signed by the server so a tampered cursor surfaces as &#x60;400&#x60; on the next call.  | [optional] 

## Example

```python
from plexsphere.models.execution_list import ExecutionList

# TODO update the JSON string below
json = "{}"
# create an instance of ExecutionList from a JSON string
execution_list_instance = ExecutionList.from_json(json)
# print the JSON string representation of the object
print(ExecutionList.to_json())

# convert the object into a dict
execution_list_dict = execution_list_instance.to_dict()
# create an instance of ExecutionList from a dict
execution_list_from_dict = ExecutionList.from_dict(execution_list_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


