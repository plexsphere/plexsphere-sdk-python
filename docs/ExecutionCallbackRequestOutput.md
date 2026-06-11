# ExecutionCallbackRequestOutput

Collected action output. Carries the inline bytes for a bounded output, or the object-store coordinates of an already-uploaded over-ceiling output. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**inline** | **bytes** | Base64-encoded inline output bytes (≤ 16 KiB). An inline body over the ceiling is refused with 413.  | [optional] 
**object_key** | **str** | Object-store key of the uploaded over-ceiling output, set on the final callback after the Node has uploaded the body to the presigned URL.  | [optional] 
**sha256** | **str** | Hex-encoded SHA-256 digest of the uploaded over-ceiling output.  | [optional] 

## Example

```python
from plexsphere.models.execution_callback_request_output import ExecutionCallbackRequestOutput

# TODO update the JSON string below
json = "{}"
# create an instance of ExecutionCallbackRequestOutput from a JSON string
execution_callback_request_output_instance = ExecutionCallbackRequestOutput.from_json(json)
# print the JSON string representation of the object
print(ExecutionCallbackRequestOutput.to_json())

# convert the object into a dict
execution_callback_request_output_dict = execution_callback_request_output_instance.to_dict()
# create an instance of ExecutionCallbackRequestOutput from a dict
execution_callback_request_output_from_dict = ExecutionCallbackRequestOutput.from_dict(execution_callback_request_output_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


