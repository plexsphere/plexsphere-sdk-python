# ProjectClusterAssignmentList

The project-id-ordered set of Project assignments placed on a management cluster, returned by `GET /v1/management-clusters/{id}/assignments`. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**items** | [**List[ProjectClusterAssignmentResponse]**](ProjectClusterAssignmentResponse.md) | Project assignments on the cluster. | 

## Example

```python
from plexsphere.models.project_cluster_assignment_list import ProjectClusterAssignmentList

# TODO update the JSON string below
json = "{}"
# create an instance of ProjectClusterAssignmentList from a JSON string
project_cluster_assignment_list_instance = ProjectClusterAssignmentList.from_json(json)
# print the JSON string representation of the object
print(ProjectClusterAssignmentList.to_json())

# convert the object into a dict
project_cluster_assignment_list_dict = project_cluster_assignment_list_instance.to_dict()
# create an instance of ProjectClusterAssignmentList from a dict
project_cluster_assignment_list_from_dict = ProjectClusterAssignmentList.from_dict(project_cluster_assignment_list_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


