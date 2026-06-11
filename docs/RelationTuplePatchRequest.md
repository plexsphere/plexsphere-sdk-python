# RelationTuplePatchRequest

Body for `PATCH /v1/authz/relation-tuples/{id}`. The handler implements Delete-then-Write semantics, so the body is the FULL replacement tuple — there are no partially-mutable fields. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**subject** | **str** | New object reference of the subject side. | 
**relation** | **str** | New relation name. | 
**resource** | **str** | New object reference of the resource side. | 
**caveat_name** | **str** | Optional caveat program name to attach to the replacement tuple. When set, SpiceDB evaluates the caveat against the supplied &#x60;caveat_context&#x60; at every Check.  | [optional] 
**caveat_context** | **Dict[str, object]** | Optional set of caveat field NAMES the new tuple binds. NAMES only — values never cross the contract boundary.  | [optional] 

## Example

```python
from plexsphere.models.relation_tuple_patch_request import RelationTuplePatchRequest

# TODO update the JSON string below
json = "{}"
# create an instance of RelationTuplePatchRequest from a JSON string
relation_tuple_patch_request_instance = RelationTuplePatchRequest.from_json(json)
# print the JSON string representation of the object
print(RelationTuplePatchRequest.to_json())

# convert the object into a dict
relation_tuple_patch_request_dict = relation_tuple_patch_request_instance.to_dict()
# create an instance of RelationTuplePatchRequest from a dict
relation_tuple_patch_request_from_dict = RelationTuplePatchRequest.from_dict(relation_tuple_patch_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


