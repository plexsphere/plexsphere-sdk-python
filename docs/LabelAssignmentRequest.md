# LabelAssignmentRequest

Body for PUT /v1/objects/{kind}/{id}/labels. Identifies the Label Definition by id and supplies the value to upsert . 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**definition_id** | **UUID** | Parent Label Definition. DECISION: addressed by id rather than qualified key — the transport resolves qualified keys to ids at decode time, and Assignment.Put must round-trip the exact aggregate that produced the snapshot.  | 
**value** | **object** |  | 

## Example

```python
from plexsphere.models.label_assignment_request import LabelAssignmentRequest

# TODO update the JSON string below
json = "{}"
# create an instance of LabelAssignmentRequest from a JSON string
label_assignment_request_instance = LabelAssignmentRequest.from_json(json)
# print the JSON string representation of the object
print(LabelAssignmentRequest.to_json())

# convert the object into a dict
label_assignment_request_dict = label_assignment_request_instance.to_dict()
# create an instance of LabelAssignmentRequest from a dict
label_assignment_request_from_dict = LabelAssignmentRequest.from_dict(label_assignment_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


