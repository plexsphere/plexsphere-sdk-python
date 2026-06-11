# LabelAssignmentList

Array wrapper for the Label Assignments attached to an object . 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**items** | [**List[LabelAssignment]**](LabelAssignment.md) |  | 

## Example

```python
from plexsphere.models.label_assignment_list import LabelAssignmentList

# TODO update the JSON string below
json = "{}"
# create an instance of LabelAssignmentList from a JSON string
label_assignment_list_instance = LabelAssignmentList.from_json(json)
# print the JSON string representation of the object
print(LabelAssignmentList.to_json())

# convert the object into a dict
label_assignment_list_dict = label_assignment_list_instance.to_dict()
# create an instance of LabelAssignmentList from a dict
label_assignment_list_from_dict = LabelAssignmentList.from_dict(label_assignment_list_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


