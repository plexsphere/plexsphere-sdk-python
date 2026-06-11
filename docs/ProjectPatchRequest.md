# ProjectPatchRequest

Body for `PATCH /v1/projects/{id}`. All properties are optional — but the body MUST set at least one of `name`, `description`, `sub_range_cidr`, or `release_sub_range`. An empty body is rejected at the handler with `400 empty_patch`.  x-project-slug-immutable: The `slug` is intentionally absent from this schema. The handler rejects any request body that carries a `slug` key (even with the same value) at decode time with `400 slug_immutable`. See the `tenancy` tag description for the rationale. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**name** | **str** | New human-readable Project name. The aggregate&#39;s &#x60;Rename&#x60; mutator validates the same constraints as &#x60;NewProject&#x60;.  | [optional] 
**description** | **str** | New free-form description. The empty string clears the description; a whitespace-only string is rejected so an operator typo cannot silently wipe the field.  | [optional] 
**sub_range_cidr** | **str** | New canonical RFC 4632 sub-range reservation. Triggers the in-tx sibling-overlap guard inside the parent Domain (&#x60;409 sub_range_overlap&#x60; / &#x60;422 sub_range_invalidates_allocation&#x60;).  | [optional] 
**release_sub_range** | **bool** | When &#x60;true&#x60;, clears the existing sub-range reservation. Mutually exclusive with &#x60;sub_range_cidr&#x60; — the service rejects bodies that carry both with &#x60;422 sub_range_invalidates_allocation&#x60;.  | [optional] 

## Example

```python
from plexsphere.models.project_patch_request import ProjectPatchRequest

# TODO update the JSON string below
json = "{}"
# create an instance of ProjectPatchRequest from a JSON string
project_patch_request_instance = ProjectPatchRequest.from_json(json)
# print the JSON string representation of the object
print(ProjectPatchRequest.to_json())

# convert the object into a dict
project_patch_request_dict = project_patch_request_instance.to_dict()
# create an instance of ProjectPatchRequest from a dict
project_patch_request_from_dict = ProjectPatchRequest.from_dict(project_patch_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


