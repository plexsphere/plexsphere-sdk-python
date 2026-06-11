# ProjectChildCounts

Per-aggregate count of children still attached to a Project at the moment a `DeleteProject` call ran. Returned in the Problem detail of a `409 project_not_empty` response so the operator knows exactly which sub-aggregate is blocking the delete . 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**resources** | **int** | Number of &#x60;Resource&#x60; aggregates still attached. | 
**nodes** | **int** | Number of &#x60;Node&#x60; aggregates still attached. | 
**relation_tuples** | **int** | Number of relation tuples still referencing the Project. | 

## Example

```python
from plexsphere.models.project_child_counts import ProjectChildCounts

# TODO update the JSON string below
json = "{}"
# create an instance of ProjectChildCounts from a JSON string
project_child_counts_instance = ProjectChildCounts.from_json(json)
# print the JSON string representation of the object
print(ProjectChildCounts.to_json())

# convert the object into a dict
project_child_counts_dict = project_child_counts_instance.to_dict()
# create an instance of ProjectChildCounts from a dict
project_child_counts_from_dict = ProjectChildCounts.from_dict(project_child_counts_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


