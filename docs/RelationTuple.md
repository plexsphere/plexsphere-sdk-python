# RelationTuple

Hydrated projection of a relation tuple aggregate. The shape is shared by `CreateRelationTuple`, `PatchRelationTuple`, and `ListRelationTuples` so clients only need one binding. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**id** | **UUID** | Relation tuple identifier (UUIDv7). | 
**subject** | **str** | Object reference of the subject side. | 
**relation** | **str** | Relation name (e.g. &#x60;maintainer&#x60;, &#x60;read&#x60;). | 
**resource** | **str** | Object reference of the resource side. | 
**caveat_context** | **Dict[str, object]** | Optional set of caveat field NAMES the tuple binds. NAMES only — values never cross the contract boundary.  | [optional] 
**created_at** | **datetime** | Aggregate creation timestamp (UTC). | 

## Example

```python
from plexsphere.models.relation_tuple import RelationTuple

# TODO update the JSON string below
json = "{}"
# create an instance of RelationTuple from a JSON string
relation_tuple_instance = RelationTuple.from_json(json)
# print the JSON string representation of the object
print(RelationTuple.to_json())

# convert the object into a dict
relation_tuple_dict = relation_tuple_instance.to_dict()
# create an instance of RelationTuple from a dict
relation_tuple_from_dict = RelationTuple.from_dict(relation_tuple_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


