# ObjectSearchResultItem

A single object reference returned by POST /v1/objects/search — the lowercase object-kind discriminator and the object UUID. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**kind** | **str** | Lowercase object-kind discriminator (e.g. &#x60;project&#x60;, &#x60;node&#x60;). | 
**id** | **UUID** | Object UUID. | 

## Example

```python
from plexsphere.models.object_search_result_item import ObjectSearchResultItem

# TODO update the JSON string below
json = "{}"
# create an instance of ObjectSearchResultItem from a JSON string
object_search_result_item_instance = ObjectSearchResultItem.from_json(json)
# print the JSON string representation of the object
print(ObjectSearchResultItem.to_json())

# convert the object into a dict
object_search_result_item_dict = object_search_result_item_instance.to_dict()
# create an instance of ObjectSearchResultItem from a dict
object_search_result_item_from_dict = ObjectSearchResultItem.from_dict(object_search_result_item_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


