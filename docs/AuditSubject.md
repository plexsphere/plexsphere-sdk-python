# AuditSubject

Subject-side projection on an audit row. `pseudonym` is the per-Domain HMAC of the subject identity — the chain-input form, never plaintext. The `identity_id_ref` is a pointer to the live Identity row when one is still resolvable; it is `null` after the right-to- erasure workflow has purged the `audit_subject_pii` mapping for the subject. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**pseudonym** | **str** | Per-Domain pseudonym of the subject (32 bytes, lowercase hex). Stable for the lifetime of the chain and never reversible to plaintext from the chain alone.  | 
**identity_id_ref** | **UUID** | Live Identity id the pseudonym maps to, or &#x60;null&#x60; if the mapping has been erased. Cleared by &#x60;EraseIdentityFromAudit&#x60; .  | [optional] 

## Example

```python
from plexsphere.models.audit_subject import AuditSubject

# TODO update the JSON string below
json = "{}"
# create an instance of AuditSubject from a JSON string
audit_subject_instance = AuditSubject.from_json(json)
# print the JSON string representation of the object
print(AuditSubject.to_json())

# convert the object into a dict
audit_subject_dict = audit_subject_instance.to_dict()
# create an instance of AuditSubject from a dict
audit_subject_from_dict = AuditSubject.from_dict(audit_subject_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


