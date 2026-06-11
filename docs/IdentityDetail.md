# IdentityDetail

Auditor-facing projection of a Domain principal returned by `GET /v1/domains/{id}/identities/{principalId}`. Extends `IdentitySummary` with optional `external_subject` and `email` plaintext fields. The plaintext fields are populated ONLY when the calling principal carries the `auditor` relation on the addressed Domain (`domain:<id>#auditor`); a `read`-only caller receives the same shape with the plaintext fields elided so a client cannot escalate by reading the wire bytes. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**id** | **UUID** | Stable principal identifier (UUIDv7), matching &#x60;IdentitySummary.id&#x60;.  | 
**kind** | [**IdentityKind**](IdentityKind.md) |  | 
**domain_id** | **UUID** | Owning Domain identifier (UUIDv7). | 
**display_name** | **str** | Operator-facing display string, matching &#x60;IdentitySummary.display_name&#x60;.  | 
**external_subject_pseudonym** | **str** | Per-Domain pseudonym of the IdP-side &#x60;external_subject&#x60; (32 bytes, lowercase hex), matching &#x60;IdentitySummary.external_subject_pseudonym&#x60;.  | 
**external_subject** | **str** | Plaintext IdP-side &#x60;external_subject&#x60; (the OIDC &#x60;sub&#x60; claim or service identity token name). Populated ONLY for callers who carry the &#x60;auditor&#x60; ReBAC relation on the addressed Domain; non-auditor callers see this field elided from the response body so the wire bytes never carry raw PII to a &#x60;read&#x60;-only caller.  | [optional] 
**email** | **str** | Plaintext IdP-side email address. Populated ONLY for callers who carry the &#x60;auditor&#x60; ReBAC relation on the addressed Domain. Service identities do not carry an &#x60;email&#x60; value and the field is omitted in that case regardless of the caller&#39;s relation.  | [optional] 
**last_sign_in_at** | **datetime** | Timestamp of the most recent successful sign-in, matching &#x60;IdentitySummary.last_sign_in_at&#x60; semantics.  | [optional] 
**created_at** | **datetime** | Aggregate creation timestamp (UTC). | 
**updated_at** | **datetime** | Last-modified timestamp (UTC). Bumped by every aggregate mutator, including &#x60;last_sign_in_at&#x60; updates from the OIDC sign-in callback.  | 

## Example

```python
from plexsphere.models.identity_detail import IdentityDetail

# TODO update the JSON string below
json = "{}"
# create an instance of IdentityDetail from a JSON string
identity_detail_instance = IdentityDetail.from_json(json)
# print the JSON string representation of the object
print(IdentityDetail.to_json())

# convert the object into a dict
identity_detail_dict = identity_detail_instance.to_dict()
# create an instance of IdentityDetail from a dict
identity_detail_from_dict = IdentityDetail.from_dict(identity_detail_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


