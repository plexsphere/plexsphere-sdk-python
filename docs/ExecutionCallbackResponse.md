# ExecutionCallbackResponse

Body of a successful `PostNodeExecutionCallback`. Carries the new invocation status, and — only on the FIRST over-ceiling callback — the presigned object-store PUT URL the Node uploads the full output to. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**status** | [**ExecutionStatus**](ExecutionStatus.md) |  | 
**output_upload_url** | **str** | Presigned object-store PUT URL the Node uploads its over- ceiling output to. Present ONLY on the first callback that declares an over-ceiling output; omitted on every other callback.  | [optional] 

## Example

```python
from plexsphere.models.execution_callback_response import ExecutionCallbackResponse

# TODO update the JSON string below
json = "{}"
# create an instance of ExecutionCallbackResponse from a JSON string
execution_callback_response_instance = ExecutionCallbackResponse.from_json(json)
# print the JSON string representation of the object
print(ExecutionCallbackResponse.to_json())

# convert the object into a dict
execution_callback_response_dict = execution_callback_response_instance.to_dict()
# create an instance of ExecutionCallbackResponse from a dict
execution_callback_response_from_dict = ExecutionCallbackResponse.from_dict(execution_callback_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


