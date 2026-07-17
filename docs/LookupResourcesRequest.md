# LookupResourcesRequest

Body for `POST /v1/authz/lookup-resources`. Enumerates the resources of `resource_type` the `subject` can reach via `relation`. An optional `caveat_context` carries the CEL caveat evaluation context (field name to value) forwarded to the authorizer. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**subject** | **str** | Object reference of the subject whose reachable resources are enumerated (e.g. &#x60;user:0190a8b8-...&#x60;, &#x60;service-identity:...&#x60;).  | 
**relation** | **str** | Relation name to evaluate (e.g. &#x60;read&#x60;, &#x60;manage&#x60;). The accepted set is fixed by the schema in &#x60;schema/authz.zed&#x60;.  | 
**resource_type** | **str** | Object type whose instances are enumerated (e.g. &#x60;project&#x60;, &#x60;domain&#x60;). Returned &#x60;items&#x60; are &#x60;&lt;resource_type&gt;:&lt;id&gt;&#x60; object references.  | 
**caveat_context** | **Dict[str, object]** | Optional CEL caveat evaluation context — a map from caveat field NAME to VALUE — forwarded verbatim to the authorizer so a caveated relation can be evaluated at check time. Values DO cross this boundary (they are the evaluation inputs); it is the AUDIT projection of this call that records field NAMES only, never the values.  | [optional] 

## Example

```python
from plexsphere.models.lookup_resources_request import LookupResourcesRequest

# TODO update the JSON string below
json = "{}"
# create an instance of LookupResourcesRequest from a JSON string
lookup_resources_request_instance = LookupResourcesRequest.from_json(json)
# print the JSON string representation of the object
print(LookupResourcesRequest.to_json())

# convert the object into a dict
lookup_resources_request_dict = lookup_resources_request_instance.to_dict()
# create an instance of LookupResourcesRequest from a dict
lookup_resources_request_from_dict = LookupResourcesRequest.from_dict(lookup_resources_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


