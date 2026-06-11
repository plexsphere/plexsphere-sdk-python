# LabelAssignment

Response shape echoing a persisted Label Assignment aggregate . 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**definition_id** | **UUID** | Parent Label Definition identifier (UUIDv7). | 
**qualified_key** | **str** | Fully-qualified Label key (denormalised). | 
**object_kind** | **str** | Lowercase object-kind discriminator. | 
**object_id** | **UUID** | Object identifier (UUIDv7). | 
**value** | **object** |  | 
**assigned_by** | **str** | Provenance marker — &#x60;user&#x60; for human callers, &#x60;system&#x60; for seed Assignments.  | 
**assigned_at** | **datetime** | RFC 3339 timestamp of the most recent write. | 

## Example

```python
from plexsphere.models.label_assignment import LabelAssignment

# TODO update the JSON string below
json = "{}"
# create an instance of LabelAssignment from a JSON string
label_assignment_instance = LabelAssignment.from_json(json)
# print the JSON string representation of the object
print(LabelAssignment.to_json())

# convert the object into a dict
label_assignment_dict = label_assignment_instance.to_dict()
# create an instance of LabelAssignment from a dict
label_assignment_from_dict = LabelAssignment.from_dict(label_assignment_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


