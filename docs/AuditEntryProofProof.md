# AuditEntryProofProof

Forensic proof material for off-line verification.

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**prev_hash** | **str** | Same value as &#x60;entry.prev_hash&#x60; (lowercase hex). | 
**entry_hash** | **str** | Same value as &#x60;entry.entry_hash&#x60; (lowercase hex). | 
**canonical_bytes** | **bytes** | Canonical-encoded chain input for this row, base64- encoded. Re-hashing &#x60;sha256(prev_hash || sha256(canonical_bytes))&#x60; reproduces &#x60;entry_hash&#x60; byte-for-byte.  | 

## Example

```python
from plexsphere.models.audit_entry_proof_proof import AuditEntryProofProof

# TODO update the JSON string below
json = "{}"
# create an instance of AuditEntryProofProof from a JSON string
audit_entry_proof_proof_instance = AuditEntryProofProof.from_json(json)
# print the JSON string representation of the object
print(AuditEntryProofProof.to_json())

# convert the object into a dict
audit_entry_proof_proof_dict = audit_entry_proof_proof_instance.to_dict()
# create an instance of AuditEntryProofProof from a dict
audit_entry_proof_proof_from_dict = AuditEntryProofProof.from_dict(audit_entry_proof_proof_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


