# AuditEraseIdentityResponse

Idempotent erasure-recorded response. `subject_pseudonym` is returned so operators can correlate the self-audit `audit.erase-identity` entry without re-deriving it. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**subject_pseudonym** | **str** | Per-Domain pseudonym derived from &#x60;identity_id&#x60; (32 bytes, lowercase hex).  | 
**erased_at** | **datetime** | Server-side timestamp the erasure was recorded. Reflects the time the self-audit &#x60;audit.erase-identity&#x60; entry was appended, not the wall-clock time of the original &#x60;audit_subject_pii&#x60; row&#39;s birth.  | 

## Example

```python
from plexsphere.models.audit_erase_identity_response import AuditEraseIdentityResponse

# TODO update the JSON string below
json = "{}"
# create an instance of AuditEraseIdentityResponse from a JSON string
audit_erase_identity_response_instance = AuditEraseIdentityResponse.from_json(json)
# print the JSON string representation of the object
print(AuditEraseIdentityResponse.to_json())

# convert the object into a dict
audit_erase_identity_response_dict = audit_erase_identity_response_instance.to_dict()
# create an instance of AuditEraseIdentityResponse from a dict
audit_erase_identity_response_from_dict = AuditEraseIdentityResponse.from_dict(audit_erase_identity_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


