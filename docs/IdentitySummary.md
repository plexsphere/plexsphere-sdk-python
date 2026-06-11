# IdentitySummary

Read-side projection of a Domain principal — user OR service identity — exposed on the listing surface. The summary intentionally omits plaintext `external_subject` and `email` and instead carries `external_subject_pseudonym` so a `domain:<id>#read` caller without the `auditor` relation never observes raw IdP-side PII. Auditor callers receive the richer `IdentityDetail` projection on the by-id surface. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**id** | **UUID** | Stable principal identifier (UUIDv7). For users this is the User aggregate id; for service identities the ServiceIdentity aggregate id.  | 
**kind** | [**IdentityKind**](IdentityKind.md) |  | 
**domain_id** | **UUID** | Owning Domain identifier (UUIDv7). | 
**display_name** | **str** | Operator-facing display string. For users this is the IdP- sourced full name; for service identities it is the operator-supplied label.  | 
**external_subject_pseudonym** | **str** | Per-Domain pseudonym of the IdP-side &#x60;external_subject&#x60; (32 bytes, lowercase hex). Always present so the listing surface is queryable without exposing plaintext PII to non-auditor callers.  | 
**last_sign_in_at** | **datetime** | Timestamp of the most recent successful sign-in for this principal, or &#x60;null&#x60; when the principal has never signed in (typical for service identities and freshly invited users).  | [optional] 
**created_at** | **datetime** | Aggregate creation timestamp (UTC). | 

## Example

```python
from plexsphere.models.identity_summary import IdentitySummary

# TODO update the JSON string below
json = "{}"
# create an instance of IdentitySummary from a JSON string
identity_summary_instance = IdentitySummary.from_json(json)
# print the JSON string representation of the object
print(IdentitySummary.to_json())

# convert the object into a dict
identity_summary_dict = identity_summary_instance.to_dict()
# create an instance of IdentitySummary from a dict
identity_summary_from_dict = IdentitySummary.from_dict(identity_summary_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


