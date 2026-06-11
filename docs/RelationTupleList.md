# RelationTupleList

Page of relation tuples returned by `GET /v1/authz/relation-tuples`. Per-row visibility is layered on top of the persistence-level page so `items` is the subset the caller is authorised to see. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**items** | [**List[RelationTuple]**](RelationTuple.md) | Relation tuples in the current page. | 
**next_cursor** | **str** | Continuation token for the next page. &#x60;null&#x60; or omitted when the iteration has reached end-of-stream. The encoding is HMAC-signed by the server so a tampered cursor surfaces as &#x60;400&#x60; on the next call.  | [optional] 

## Example

```python
from plexsphere.models.relation_tuple_list import RelationTupleList

# TODO update the JSON string below
json = "{}"
# create an instance of RelationTupleList from a JSON string
relation_tuple_list_instance = RelationTupleList.from_json(json)
# print the JSON string representation of the object
print(RelationTupleList.to_json())

# convert the object into a dict
relation_tuple_list_dict = relation_tuple_list_instance.to_dict()
# create an instance of RelationTupleList from a dict
relation_tuple_list_from_dict = RelationTupleList.from_dict(relation_tuple_list_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


