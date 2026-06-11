# InvitationCreateRequest

Body for `POST /v1/domains/{id}/invitations`. The handler authorises the call against the parent Domain's `manage` ReBAC relation BEFORE invoking the service so an unauthorised caller never produces an `InvitationCreated` outbox row. The plaintext `external_subject` is intentionally NOT echoed in the response — only the per-Domain pseudonym is — so a server-side log of the response body never carries raw IdP-side PII. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**external_subject** | **str** | IdP-side subject identifier (typically the OIDC &#x60;sub&#x60; claim or operator-known login name) the invitation is staged for. Whitespace-only is rejected by the aggregate .  | 
**ttl_seconds** | **int** | Lifetime of the invitation in seconds, applied to &#x60;created_at&#x60; to derive &#x60;expires_at&#x60;. Out-of-range values surface as &#x60;400 invalid_ttl&#x60;. Defaults to 86400 (24h); ceiling is 7 days.  | [optional] [default to 86400]
**initial_tuples** | [**List[InvitationInitialTuple]**](InvitationInitialTuple.md) | Optional bounded list (max 32) of relation tuples to write into SpiceDB atomically when the invitation is accepted. Exceeding the cap surfaces as &#x60;422 too_many_initial_tuples&#x60;.  | [optional] 

## Example

```python
from plexsphere.models.invitation_create_request import InvitationCreateRequest

# TODO update the JSON string below
json = "{}"
# create an instance of InvitationCreateRequest from a JSON string
invitation_create_request_instance = InvitationCreateRequest.from_json(json)
# print the JSON string representation of the object
print(InvitationCreateRequest.to_json())

# convert the object into a dict
invitation_create_request_dict = invitation_create_request_instance.to_dict()
# create an instance of InvitationCreateRequest from a dict
invitation_create_request_from_dict = InvitationCreateRequest.from_dict(invitation_create_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


