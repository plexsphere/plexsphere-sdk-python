# InvitationResponse

Hydrated projection of an Invitation aggregate. The shape is shared by `CreateInvitation`, `GetInvitation`, and `ListInvitations`. Note that the plaintext `external_subject` is intentionally absent — the wire shape only carries the per-Domain pseudonym so a `read`-only caller never observes raw IdP-side PII through the invitation surface. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**id** | **UUID** | Invitation identifier (UUIDv7). | 
**domain_id** | **UUID** | Owning Domain identifier (UUIDv7). | 
**external_subject_pseudonym** | **str** | Per-Domain pseudonym of the IdP-side &#x60;external_subject&#x60; (32 bytes, lowercase hex). The plaintext form is never echoed on the invitation read surface.  | 
**status** | [**InvitationStatus**](InvitationStatus.md) |  | 
**expires_at** | **datetime** | Wall-clock timestamp the invitation transitions to &#x60;expired&#x60; if no Accept / Revoke has fired. Derived from &#x60;created_at + ttl_seconds&#x60;. The expiry sweeper flips elapsed pending rows to &#x60;expired&#x60; and appends the matching outbox event.  | 
**created_at** | **datetime** | Aggregate creation timestamp (UTC). | 
**accepted_at** | **datetime** | Timestamp the invitation transitioned to &#x60;accepted&#x60;. Populated only when &#x60;status &#x3D;&#x3D; accepted&#x60;.  | [optional] 
**accepted_user_id** | **UUID** | Identifier of the User aggregate the OIDC sign-in callback resolved when accepting the invitation. Populated only when &#x60;status &#x3D;&#x3D; accepted&#x60;.  | [optional] 
**revoked_at** | **datetime** | Timestamp the invitation transitioned to &#x60;revoked&#x60;. Populated only when &#x60;status &#x3D;&#x3D; revoked&#x60;.  | [optional] 
**expired_at** | **datetime** | Timestamp the expiry sweeper flipped the row to &#x60;expired&#x60;. Populated only when &#x60;status &#x3D;&#x3D; expired&#x60; .  | [optional] 
**initial_tuples** | [**List[InvitationInitialTuple]**](InvitationInitialTuple.md) | Bounded list of relation tuples staged on the invitation. Returned verbatim from the persistence layer so the operator can preview which tuples will land on Accept .  | [optional] 

## Example

```python
from plexsphere.models.invitation_response import InvitationResponse

# TODO update the JSON string below
json = "{}"
# create an instance of InvitationResponse from a JSON string
invitation_response_instance = InvitationResponse.from_json(json)
# print the JSON string representation of the object
print(InvitationResponse.to_json())

# convert the object into a dict
invitation_response_dict = invitation_response_instance.to_dict()
# create an instance of InvitationResponse from a dict
invitation_response_from_dict = InvitationResponse.from_dict(invitation_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


