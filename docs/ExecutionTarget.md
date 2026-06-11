# ExecutionTarget

Per-Node invocation projection — one target the Execution fanned out to, with its current status and (once collected) a pointer to the output it produced. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**node_id** | **UUID** | Identifier of the target Node (UUIDv7). | 
**status** | [**ExecutionStatus**](ExecutionStatus.md) |  | 
**created_at** | **datetime** | Target creation timestamp (UTC). | 
**updated_at** | **datetime** | Timestamp of the target&#39;s most recent status advance (UTC). | 
**output_ref** | [**OutputRef**](OutputRef.md) | Pointer to the output the invocation produced. Present only once the target reaches a terminal status that collected output; omitted otherwise.  | [optional] 

## Example

```python
from plexsphere.models.execution_target import ExecutionTarget

# TODO update the JSON string below
json = "{}"
# create an instance of ExecutionTarget from a JSON string
execution_target_instance = ExecutionTarget.from_json(json)
# print the JSON string representation of the object
print(ExecutionTarget.to_json())

# convert the object into a dict
execution_target_dict = execution_target_instance.to_dict()
# create an instance of ExecutionTarget from a dict
execution_target_from_dict = ExecutionTarget.from_dict(execution_target_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


