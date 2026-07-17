# ObjectSearchResponse

Result of POST /v1/objects/search. `items` is the page of object references the selector matched AND the caller can reach via the requested relation; an empty array is a normal answer. `next_cursor` is empty on the last page. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**items** | [**List[ObjectSearchResultItem]**](ObjectSearchResultItem.md) | Page of matched, access-checked object references. May be shorter than the requested &#x60;limit&#x60; because the ReBAC filter runs after pagination.  | 
**next_cursor** | **str** | Opaque cursor for the next page, or empty when this is the last page.  | [optional] 
**correlation_id** | **str** | Correlation id pairing this search with the matching audit entry emitted by &#x60;internal/audit&#x60;.  | 

## Example

```python
from plexsphere.models.object_search_response import ObjectSearchResponse

# TODO update the JSON string below
json = "{}"
# create an instance of ObjectSearchResponse from a JSON string
object_search_response_instance = ObjectSearchResponse.from_json(json)
# print the JSON string representation of the object
print(ObjectSearchResponse.to_json())

# convert the object into a dict
object_search_response_dict = object_search_response_instance.to_dict()
# create an instance of ObjectSearchResponse from a dict
object_search_response_from_dict = ObjectSearchResponse.from_dict(object_search_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


