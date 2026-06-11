# GroupRequest

Body for POST /v1/admin/groups. Field set mirrors the `Group` aggregate. DECISION: the aggregate enforces a source-specific invariant at construction time — `source=idp` requires both `idp_binding_id` (non-zero) AND `idp_claim_value` (non-empty); `source=manual` forbids both. Violations are rejected with a 400 Problem from validation rather than by a SQL CHECK constraint, so the failure surfaces in the transport layer rather than the persistence layer. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**domain_id** | **UUID** | Owning Domain. | 
**slug** | **str** | URL-safe identifier unique within &#x60;domain_id&#x60;. Lowercase ASCII letters, digits, and internal dashes only; cannot start or end with a dash.  | 
**display_name** | **str** | Human-friendly Group name. | 
**source** | **str** | Provenance of the Group. &#x60;manual&#x60; means the Group was created via this admin API; &#x60;idp&#x60; means it is reconciled from an IdP claim. DECISION: &#x60;source&#x3D;idp&#x60; requires &#x60;idp_binding_id&#x60; and &#x60;idp_claim_value&#x60;; &#x60;source&#x3D;manual&#x60; forbids both. The aggregate rejects mismatches so the error surfaces as 400, not as a SQL CHECK violation .  | 
**idp_binding_id** | **UUID** | IdP binding the Group is reconciled from. Required when &#x60;source&#x3D;idp&#x60;; must be omitted when &#x60;source&#x3D;manual&#x60; .  | [optional] 
**idp_claim_value** | **str** | Verbatim IdP claim value the Group reconciles to. Required when &#x60;source&#x3D;idp&#x60;; must be omitted when &#x60;source&#x3D;manual&#x60; .  | [optional] 

## Example

```python
from plexsphere.models.group_request import GroupRequest

# TODO update the JSON string below
json = "{}"
# create an instance of GroupRequest from a JSON string
group_request_instance = GroupRequest.from_json(json)
# print the JSON string representation of the object
print(GroupRequest.to_json())

# convert the object into a dict
group_request_dict = group_request_instance.to_dict()
# create an instance of GroupRequest from a dict
group_request_from_dict = GroupRequest.from_dict(group_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


