# RelationTupleCreateRequest

Body for `POST /v1/authz/relation-tuples`. Field set mirrors the relation-tuple aggregate's `NewRelationTuple` invariants. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**subject** | **str** | Object reference of the subject side. | 
**relation** | **str** | Relation name. | 
**resource** | **str** | Object reference of the resource side. | 
**caveat_name** | **str** | Optional caveat program name to attach to the tuple. When set, SpiceDB evaluates the caveat against the supplied &#x60;caveat_context&#x60; at every Check.  | [optional] 
**caveat_context** | **Dict[str, object]** | Optional set of caveat field NAMES the tuple binds. NAMES only — values never cross the contract boundary.  | [optional] 

## Example

```python
from plexsphere.models.relation_tuple_create_request import RelationTupleCreateRequest

# TODO update the JSON string below
json = "{}"
# create an instance of RelationTupleCreateRequest from a JSON string
relation_tuple_create_request_instance = RelationTupleCreateRequest.from_json(json)
# print the JSON string representation of the object
print(RelationTupleCreateRequest.to_json())

# convert the object into a dict
relation_tuple_create_request_dict = relation_tuple_create_request_instance.to_dict()
# create an instance of RelationTupleCreateRequest from a dict
relation_tuple_create_request_from_dict = RelationTupleCreateRequest.from_dict(relation_tuple_create_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


