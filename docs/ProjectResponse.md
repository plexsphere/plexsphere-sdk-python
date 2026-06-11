# ProjectResponse

Hydrated projection of a tenancy Project aggregate. The shape is shared by `CreateProject`, `GetProject`, `ListProjects`, and `PatchProject` — every read surface returns the same wire bytes for the same Project so clients only need one binding. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**id** | **UUID** | Project identifier (UUIDv7). | 
**domain_id** | **UUID** | Parent Domain identifier (UUIDv7). | 
**name** | **str** | Human-readable Project name (trimmed at the aggregate). | 
**slug** | **str** | Kebab-case URL handle. Stable for the lifetime of the Project — see the &#x60;tenancy&#x60; tag description for the immutability rationale.  | 
**description** | **str** | Optional free-form description. Empty string when the operator did not set one (the wire shape never carries &#x60;null&#x60; for this field).  | [optional] 
**sub_range_cidr** | **str** | Optional canonical RFC 4632 sub-range reservation inside the parent Domain&#39;s mesh_cidr. Cross-Project non-overlap within the parent Domain is enforced by the SQL GIST exclusion constraint on &#x60;plexsphere.project_mesh_ip_reservations&#x60;.  | [optional] 
**created_at** | **datetime** | Aggregate creation timestamp (UTC). | 
**updated_at** | **datetime** | Last-modified timestamp (UTC). Bumped by every mutator — &#x60;Rename&#x60;, &#x60;ChangeDescription&#x60;, &#x60;ReserveSubRange&#x60;, &#x60;ResizeSubRange&#x60;, &#x60;ReleaseSubRange&#x60;.  | 

## Example

```python
from plexsphere.models.project_response import ProjectResponse

# TODO update the JSON string below
json = "{}"
# create an instance of ProjectResponse from a JSON string
project_response_instance = ProjectResponse.from_json(json)
# print the JSON string representation of the object
print(ProjectResponse.to_json())

# convert the object into a dict
project_response_dict = project_response_instance.to_dict()
# create an instance of ProjectResponse from a dict
project_response_from_dict = ProjectResponse.from_dict(project_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


