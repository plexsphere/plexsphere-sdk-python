# SinkList

Set of sinks returned by `GET /v1/domains/{id}/sinks` (the tenant sinks one Domain declares) and by `GET /v1/sinks/built-in` (the platform's own backends), ordered by slug. Both sets are bounded — the platform ships three built-in backends, and a Domain is refused a create past its sink ceiling with `409 sink_limit_reached` — so neither is paginated. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**items** | [**List[Sink]**](Sink.md) | The sinks in the set. | 

## Example

```python
from plexsphere.models.sink_list import SinkList

# TODO update the JSON string below
json = "{}"
# create an instance of SinkList from a JSON string
sink_list_instance = SinkList.from_json(json)
# print the JSON string representation of the object
print(SinkList.to_json())

# convert the object into a dict
sink_list_dict = sink_list_instance.to_dict()
# create an instance of SinkList from a dict
sink_list_from_dict = SinkList.from_dict(sink_list_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


