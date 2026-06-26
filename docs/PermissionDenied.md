# PermissionDenied

Specialised RFC 9457 problem-details body returned on 403 from the ReBAC authorizer. Extends `Problem` with a machine-readable denial reason, the traversed relation path, and the correlation id that pairs the response with the matching audit entry emitted by `internal/audit`.  DECISION: the schema is declared as a standalone object rather than via `allOf` against `Problem`. kin-openapi's validator and oapi-codegen both accept `allOf`, but a generated client can treat `allOf` members as intersections that collide on optional fields — duplicating the Problem fields in place keeps the generated Go client shape flat and sidesteps the collision. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**type** | **str** | URI reference that identifies the problem type. | 
**title** | **str** | Short, human-readable summary of the problem. | 
**status** | **int** | HTTP status code generated for this occurrence (always 403). | 
**detail** | **str** | Human-readable explanation for this occurrence. | 
**instance** | **str** | URI reference identifying the specific occurrence. | 
**code** | **str** | Optional machine-readable error code (shared with &#x60;Problem&#x60;).  | [optional] 
**reason** | **str** | Machine-readable denial reason. The enum values match &#x60;internal/audit.Reason.String&#x60; exactly: &#x60;out_of_scope&#x60; — the request carried no relevant subject binding for the object; &#x60;insufficient_relation&#x60; — a subject binding exists but no permission derivation grants the attempted action; &#x60;caveat_violation&#x60; — the tuple graph permitted the action but a CEL caveat (within_time_window, from_cidr, requires_assurance) evaluated to false.  | 
**relation_path** | **List[str]** | Ordered sequence of relations the authorizer traversed while evaluating the Check. Empty when the denial was decided before any relation lookup (typically &#x60;out_of_scope&#x60;).  | [optional] 
**correlation_id** | **str** | Correlation id that pairs this denial with the matching audit entry emitted by &#x60;internal/audit&#x60;.  | 

## Example

```python
from plexsphere.models.permission_denied import PermissionDenied

# TODO update the JSON string below
json = "{}"
# create an instance of PermissionDenied from a JSON string
permission_denied_instance = PermissionDenied.from_json(json)
# print the JSON string representation of the object
print(PermissionDenied.to_json())

# convert the object into a dict
permission_denied_dict = permission_denied_instance.to_dict()
# create an instance of PermissionDenied from a dict
permission_denied_from_dict = PermissionDenied.from_dict(permission_denied_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


