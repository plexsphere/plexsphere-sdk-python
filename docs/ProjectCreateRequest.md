# ProjectCreateRequest

Body for `POST /v1/projects`. Field set mirrors the Project aggregate's `NewProject` invariants. The handler authorises the call against the parent Domain's `manage` ReBAC relation BEFORE invoking the service so an unauthorised caller never produces a `ProjectCreated` outbox row. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**domain_id** | **UUID** | Parent Domain identifier (UUIDv7). The handler authorises &#x60;manage&#x60; on &#x60;domain:&lt;id&gt;&#x60; BEFORE invoking the service .  | 
**name** | **str** | Human-readable Project name. Whitespace-only is rejected. | 
**slug** | **str** | Kebab-case URL handle. The aggregate&#39;s &#x60;ParseSlug&#x60; enforces the same regex; surfacing the pattern here lets the generated client validate before the round-trip.  | 
**description** | **str** | Optional free-form description. Whitespace-only strings are rejected by the aggregate (the operator most likely fat-fingered the field instead of meaning to clear it).  | [optional] 
**sub_range_cidr** | **str** | Optional canonical RFC 4632 sub-range reservation. Must be contained in the parent Domain&#39;s mesh_cidr and must not overlap a sibling Project&#39;s reservation. Surfaces as &#x60;409 sub_range_overlap&#x60; on conflict.  | [optional] 

## Example

```python
from plexsphere.models.project_create_request import ProjectCreateRequest

# TODO update the JSON string below
json = "{}"
# create an instance of ProjectCreateRequest from a JSON string
project_create_request_instance = ProjectCreateRequest.from_json(json)
# print the JSON string representation of the object
print(ProjectCreateRequest.to_json())

# convert the object into a dict
project_create_request_dict = project_create_request_instance.to_dict()
# create an instance of ProjectCreateRequest from a dict
project_create_request_from_dict = ProjectCreateRequest.from_dict(project_create_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


