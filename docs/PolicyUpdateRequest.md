# PolicyUpdateRequest

Body for patching a Policy. Every revision carries a full `selector` + `rules` pair, so both are REQUIRED — the aggregate does not support a partial revision that inherits the head's selector or rules, and a body missing either surfaces as `400 empty_patch`. `display_name` is the only optional field. selector and rules are NOT marked `required` in this schema on purpose: the plain server needs to distinguish an ABSENT field (inherit nothing, reject as empty) from a zero-value one, which a required non-nullable field would erase — so the both-present invariant is enforced by the handler, which answers `400 empty_patch` when either is missing. Optimistic concurrency is ALWAYS enforced: the handler and the append-and-advance CAS predicate gate on the head revision, so two concurrent PATCHes deterministically produce one winner and one `409 revision_conflict`. `expected_revision_id` is the editor's \"I observed this head\" hint; omitting it does not disable the CAS — the server falls back to its own freshly observed head. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**display_name** | **str** |  | [optional] 
**selector** | [**PolicySelector**](PolicySelector.md) |  | [optional] 
**rules** | [**List[PolicyRule]**](PolicyRule.md) |  | [optional] 
**expected_revision_id** | **UUID** | Revision identifier the client believes is current. When present the handler also rejects a stale value up front; with or without it the append-and-advance CAS still forces a losing concurrent writer onto &#x60;409 revision_conflict&#x60;.  | [optional] 

## Example

```python
from plexsphere.models.policy_update_request import PolicyUpdateRequest

# TODO update the JSON string below
json = "{}"
# create an instance of PolicyUpdateRequest from a JSON string
policy_update_request_instance = PolicyUpdateRequest.from_json(json)
# print the JSON string representation of the object
print(PolicyUpdateRequest.to_json())

# convert the object into a dict
policy_update_request_dict = policy_update_request_instance.to_dict()
# create an instance of PolicyUpdateRequest from a dict
policy_update_request_from_dict = PolicyUpdateRequest.from_dict(policy_update_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


