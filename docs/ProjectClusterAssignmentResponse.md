# ProjectClusterAssignmentResponse

Read-side projection of a Project ↔ management-cluster assignment. The shape is shared by `GetProjectManagementClusterAssignment`, `ListManagementClusterAssignments`, and `TerminateProjectManagementClusterAssignment`. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**project_id** | **UUID** | Placed Project identifier (UUIDv7). | 
**management_cluster_id** | **UUID** | Identifier of the management cluster the Project is placed onto.  | 
**region** | **str** | Region the assignment landed in. | 
**namespace_name** | **str** | Derived per-Project namespace name on the management cluster (&#x60;plexsphere-project-&lt;project-uuid&gt;&#x60;).  | 
**namespace_phase** | [**NamespacePhase**](NamespacePhase.md) |  | 
**assigned_at** | **datetime** | Assignment creation timestamp (UTC). | 
**updated_at** | **datetime** | Last-modified timestamp (UTC); bumped on every phase advance.  | 

## Example

```python
from plexsphere.models.project_cluster_assignment_response import ProjectClusterAssignmentResponse

# TODO update the JSON string below
json = "{}"
# create an instance of ProjectClusterAssignmentResponse from a JSON string
project_cluster_assignment_response_instance = ProjectClusterAssignmentResponse.from_json(json)
# print the JSON string representation of the object
print(ProjectClusterAssignmentResponse.to_json())

# convert the object into a dict
project_cluster_assignment_response_dict = project_cluster_assignment_response_instance.to_dict()
# create an instance of ProjectClusterAssignmentResponse from a dict
project_cluster_assignment_response_from_dict = ProjectClusterAssignmentResponse.from_dict(project_cluster_assignment_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


