# GroupMembershipRequest

Body for POST /v1/admin/groups/{id}/members. The Membership's `source` MUST match the parent Group's `source`; the handler resolves the parent and rejects mismatches with 409 source-conflict before a SQL round-trip.  DECISION: the request body does NOT carry `idp_claim_value`. The per-Group claim mapping lives on the parent Group aggregate (`idp_claim_value` on `plexsphere.groups`); every Membership attached to an IdP-sourced Group inherits that mapping. Putting a claim on each row would be contract incoherence with the sync path (`MembershipRepo.SyncForUser` reads `parent.IdPClaimValue` exclusively). 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**principal_id** | **UUID** | Principal identifier (UUIDv7). | 
**principal_kind** | **str** | Principal discriminator. The repo uses this to target the correct XOR column on &#x60;group_memberships&#x60; .  | 
**source** | **str** | Provenance of the Membership. MUST equal the parent Group&#39;s &#x60;source&#x60;; a mismatch returns 409 source-conflict .  | 

## Example

```python
from plexsphere.models.group_membership_request import GroupMembershipRequest

# TODO update the JSON string below
json = "{}"
# create an instance of GroupMembershipRequest from a JSON string
group_membership_request_instance = GroupMembershipRequest.from_json(json)
# print the JSON string representation of the object
print(GroupMembershipRequest.to_json())

# convert the object into a dict
group_membership_request_dict = group_membership_request_instance.to_dict()
# create an instance of GroupMembershipRequest from a dict
group_membership_request_from_dict = GroupMembershipRequest.from_dict(group_membership_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


