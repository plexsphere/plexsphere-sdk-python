# AuditEntryProof

Single audit row plus the canonical-bytes proof needed to recompute its hash off-line. The verifier recomputes `sha256(prev_hash || sha256(canonical_bytes))` and compares the result against `entry_hash` row by row. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**entry** | [**AuditEntry**](AuditEntry.md) |  | 
**proof** | [**AuditEntryProofProof**](AuditEntryProofProof.md) |  | 

## Example

```python
from plexsphere.models.audit_entry_proof import AuditEntryProof

# TODO update the JSON string below
json = "{}"
# create an instance of AuditEntryProof from a JSON string
audit_entry_proof_instance = AuditEntryProof.from_json(json)
# print the JSON string representation of the object
print(AuditEntryProof.to_json())

# convert the object into a dict
audit_entry_proof_dict = audit_entry_proof_instance.to_dict()
# create an instance of AuditEntryProof from a dict
audit_entry_proof_from_dict = AuditEntryProof.from_dict(audit_entry_proof_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


