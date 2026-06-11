# StateEntry

A single node-state entry within a `NodeStateReports` bucket. The triple (`key`, `value`, `workload_tag`) is the minimum plexd needs to correlate a reported entry with the workload that produced it. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**key** | **str** | The entry&#39;s stable identity within its bucket. | 
**value** | **str** | The entry&#39;s reported value, carried opaquely. | 
**workload_tag** | **str** | Tag of the workload that produced the entry. Absent or empty when the entry is not attributed to a workload.  | [optional] 

## Example

```python
from plexsphere.models.state_entry import StateEntry

# TODO update the JSON string below
json = "{}"
# create an instance of StateEntry from a JSON string
state_entry_instance = StateEntry.from_json(json)
# print the JSON string representation of the object
print(StateEntry.to_json())

# convert the object into a dict
state_entry_dict = state_entry_instance.to_dict()
# create an instance of StateEntry from a dict
state_entry_from_dict = StateEntry.from_dict(state_entry_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


