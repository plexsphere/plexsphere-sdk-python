# AuditEraseIdentityRequest

Inbound right-to-erasure request. The endpoint is idempotent on `subject_pseudonym` — replaying the request after a successful erasure is safe. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**identity_id** | **UUID** | Identity whose &#x60;audit_subject_pii&#x60; mapping must be purged from the addressed Domain. The pseudonym is derived server-side from &#x60;(domain_id, identity_id)&#x60; and the per-Domain pepper; clients never compute it themselves .  | 

## Example

```python
from plexsphere.models.audit_erase_identity_request import AuditEraseIdentityRequest

# TODO update the JSON string below
json = "{}"
# create an instance of AuditEraseIdentityRequest from a JSON string
audit_erase_identity_request_instance = AuditEraseIdentityRequest.from_json(json)
# print the JSON string representation of the object
print(AuditEraseIdentityRequest.to_json())

# convert the object into a dict
audit_erase_identity_request_dict = audit_erase_identity_request_instance.to_dict()
# create an instance of AuditEraseIdentityRequest from a dict
audit_erase_identity_request_from_dict = AuditEraseIdentityRequest.from_dict(audit_erase_identity_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


