# InvitationInitialTuple

Relation tuple consumed atomically when an Invitation is accepted. The aggregate stores the tuples as a bounded jsonb array (max 32) on `plexsphere.invitations.initial_tuples` and rejects entries whose `object` is not in the closed prefix set `{domain:<this-id>, project:<uuid>, group:<uuid>}` — cross- Domain or platform-scoped objects surface as `422 invitation_object_out_of_scope`. The aggregate also rejects a non-JSON-encodable `caveat_context` with `422 invalid_caveat_context`. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**relation** | **str** | SpiceDB relation the tuple grants on &#x60;object&#x60; (e.g. &#x60;member&#x60;, &#x60;viewer&#x60;). Whitespace-only is rejected by the aggregate.  | 
**object** | **str** | SpiceDB object reference the tuple targets. Allowed prefix set is closed: &#x60;domain:&lt;this-domain-id&gt;&#x60;, &#x60;project:&lt;uuid&gt;&#x60;, or &#x60;group:&lt;uuid&gt;&#x60;. The handler validates the prefix BEFORE the aggregate constructor so an out-of- scope object surfaces as &#x60;422 invitation_object_out_of_scope&#x60; rather than a generic &#x60;invalid_body&#x60;.  | 
**caveat_context** | **Dict[str, object]** | Optional CEL caveat context forwarded verbatim to SpiceDB on accept. The aggregate enforces JSON-encodable lossless round-trip — a value that fails the round-trip surfaces as &#x60;422 invalid_caveat_context&#x60;.  | [optional] 

## Example

```python
from plexsphere.models.invitation_initial_tuple import InvitationInitialTuple

# TODO update the JSON string below
json = "{}"
# create an instance of InvitationInitialTuple from a JSON string
invitation_initial_tuple_instance = InvitationInitialTuple.from_json(json)
# print the JSON string representation of the object
print(InvitationInitialTuple.to_json())

# convert the object into a dict
invitation_initial_tuple_dict = invitation_initial_tuple_instance.to_dict()
# create an instance of InvitationInitialTuple from a dict
invitation_initial_tuple_from_dict = InvitationInitialTuple.from_dict(invitation_initial_tuple_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


