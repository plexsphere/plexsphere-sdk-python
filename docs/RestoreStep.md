# RestoreStep

One step of the ordered disaster-recovery restore plan: the store to restore, the action to take, and how to verify the step succeeded. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**order** | **int** | The step&#39;s position in the restore sequence; steps are contiguous ascending starting at 1.  | 
**name** | **str** | The store or component the step restores. | 
**action** | **str** | The action the operator takes for the step. | 
**verify** | **str** | How the operator verifies the step succeeded. | 

## Example

```python
from plexsphere.models.restore_step import RestoreStep

# TODO update the JSON string below
json = "{}"
# create an instance of RestoreStep from a JSON string
restore_step_instance = RestoreStep.from_json(json)
# print the JSON string representation of the object
print(RestoreStep.to_json())

# convert the object into a dict
restore_step_dict = restore_step_instance.to_dict()
# create an instance of RestoreStep from a dict
restore_step_from_dict = RestoreStep.from_dict(restore_step_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


