# ObjectSearchRequest

Body for POST /v1/objects/search. Names the Label selector to resolve, the tenancy scope to resolve it within, and the ReBAC relation each match is access-checked against. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**selector** | **str** | Raw label-selector expression (for example &#x60;env&#x3D;production, tier in (gold, silver)&#x60;). Parsed by the same grammar /v1/labels/selectors/preview validates.  | 
**relation** | **str** | ReBAC relation the caller must hold on a match for it to appear in the result (for example &#x60;read&#x60;). Required; clients commonly default it to &#x60;read&#x60;.  | 
**scope** | **str** | Tenancy universe the selector is resolved within. &#x60;platform&#x60; spans every object; &#x60;domain&#x60; and &#x60;project&#x60; narrow the match set to objects carrying at least one in-scope Label.  | 
**scope_id** | **UUID** | Domain or Project UUID addressed by &#x60;scope&#x60;. Required when &#x60;scope&#x60; is &#x60;domain&#x60; or &#x60;project&#x60;; MUST be omitted (or the zero UUID) when &#x60;scope&#x60; is &#x60;platform&#x60;.  | [optional] 
**kind** | **str** | Optional object-kind filter (for example &#x60;project&#x60; or &#x60;node&#x60;). When set, only matches of that kind are returned; when omitted the result is mixed-kind.  | [optional] 
**cursor** | **str** | Opaque forward cursor from a previous page&#39;s &#x60;next_cursor&#x60;. Omit (or send empty) to read the first page.  | [optional] 
**limit** | **int** | Upper bound on the PRE-filter page size. Because the ReBAC filter runs after pagination, the returned page may hold fewer items than &#x60;limit&#x60;; paginate until &#x60;next_cursor&#x60; is absent. Omitted defers to the server default.  | [optional] 

## Example

```python
from plexsphere.models.object_search_request import ObjectSearchRequest

# TODO update the JSON string below
json = "{}"
# create an instance of ObjectSearchRequest from a JSON string
object_search_request_instance = ObjectSearchRequest.from_json(json)
# print the JSON string representation of the object
print(ObjectSearchRequest.to_json())

# convert the object into a dict
object_search_request_dict = object_search_request_instance.to_dict()
# create an instance of ObjectSearchRequest from a dict
object_search_request_from_dict = ObjectSearchRequest.from_dict(object_search_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


