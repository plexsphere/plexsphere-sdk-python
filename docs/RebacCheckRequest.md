# RebacCheckRequest

Body for `POST /v1/authz/check`. Triple addressing `(subject, relation, resource)` mirrors the canonical ReBAC tuple shape; an optional `caveat_context` carries the field NAMES the caveat program reads — never values. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**subject** | **str** | Object reference of the subject performing the check (e.g. &#x60;user:0190a8b8-...&#x60;, &#x60;service-identity:...&#x60;).  | 
**relation** | **str** | Relation name to evaluate (e.g. &#x60;read&#x60;, &#x60;manage&#x60;, &#x60;maintainer&#x60;). The accepted set is fixed by the schema in &#x60;schema/authz.zed&#x60;.  | 
**resource** | **str** | Object reference of the resource the relation is evaluated against (e.g. &#x60;project:0190a8b8-...&#x60;, &#x60;domain:...&#x60;).  | 
**caveat_context** | **Dict[str, object]** | Optional set of caveat field NAMES the request makes available to the caveat program. Values are never carried across the contract boundary; the field type is &#x60;object&#x60; for forward-compatibility but the contract requires NAMES-only payloads.  | [optional] 

## Example

```python
from plexsphere.models.rebac_check_request import RebacCheckRequest

# TODO update the JSON string below
json = "{}"
# create an instance of RebacCheckRequest from a JSON string
rebac_check_request_instance = RebacCheckRequest.from_json(json)
# print the JSON string representation of the object
print(RebacCheckRequest.to_json())

# convert the object into a dict
rebac_check_request_dict = rebac_check_request_instance.to_dict()
# create an instance of RebacCheckRequest from a dict
rebac_check_request_from_dict = RebacCheckRequest.from_dict(rebac_check_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


