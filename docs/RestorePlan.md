# RestorePlan

The platform-scoped, ordered disaster-recovery restore plan. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**sequence** | [**List[RestoreStep]**](RestoreStep.md) | The ordered restore steps, contiguous ascending by &#x60;order&#x60; starting at 1.  | 

## Example

```python
from plexsphere.models.restore_plan import RestorePlan

# TODO update the JSON string below
json = "{}"
# create an instance of RestorePlan from a JSON string
restore_plan_instance = RestorePlan.from_json(json)
# print the JSON string representation of the object
print(RestorePlan.to_json())

# convert the object into a dict
restore_plan_dict = restore_plan_instance.to_dict()
# create an instance of RestorePlan from a dict
restore_plan_from_dict = RestorePlan.from_dict(restore_plan_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


