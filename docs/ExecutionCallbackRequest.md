# ExecutionCallbackRequest

Body a Node POSTs to `POST /v1/nodes/{id}/executions/{exec_id}` to report a status advance for its invocation. The reported `status` drives the closed state machine; the optional `output` carries the action's collected output, either inline (≤ 16 KiB) or — for an over- ceiling body — the object-store coordinates of the uploaded object. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**status** | [**ExecutionStatus**](ExecutionStatus.md) |  | 
**exit_code** | **int** | Process exit code the action reported. Present on terminal callbacks that ran the action to completion.  | [optional] 
**error** | **str** | Free-text error the action reported. Present on a &#x60;failed&#x60; terminal callback.  | [optional] 
**declared_output_bytes** | **int** | Size in bytes the Node declares it intends to upload. A value over the 16 KiB ceiling drives the over-ceiling presign mint on the first callback.  | [optional] 
**output** | [**ExecutionCallbackRequestOutput**](ExecutionCallbackRequestOutput.md) |  | [optional] 

## Example

```python
from plexsphere.models.execution_callback_request import ExecutionCallbackRequest

# TODO update the JSON string below
json = "{}"
# create an instance of ExecutionCallbackRequest from a JSON string
execution_callback_request_instance = ExecutionCallbackRequest.from_json(json)
# print the JSON string representation of the object
print(ExecutionCallbackRequest.to_json())

# convert the object into a dict
execution_callback_request_dict = execution_callback_request_instance.to_dict()
# create an instance of ExecutionCallbackRequest from a dict
execution_callback_request_from_dict = ExecutionCallbackRequest.from_dict(execution_callback_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


