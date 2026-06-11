# BreakGlassRequest

Body for `POST /v1/approvals/{id}/break-glass`. The `reason` is the mandatory emergency justification for the override. The value is PII recorded only by its field NAME on the decision's audit caveat context and routed to a PII-safe sink — it never crosses the contract boundary verbatim. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**reason** | **str** | Emergency justification. At least 16 characters — a shorter value is rejected with &#x60;400 invalid_break_glass_reason&#x60;.  | 

## Example

```python
from plexsphere.models.break_glass_request import BreakGlassRequest

# TODO update the JSON string below
json = "{}"
# create an instance of BreakGlassRequest from a JSON string
break_glass_request_instance = BreakGlassRequest.from_json(json)
# print the JSON string representation of the object
print(BreakGlassRequest.to_json())

# convert the object into a dict
break_glass_request_dict = break_glass_request_instance.to_dict()
# create an instance of BreakGlassRequest from a dict
break_glass_request_from_dict = BreakGlassRequest.from_dict(break_glass_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


