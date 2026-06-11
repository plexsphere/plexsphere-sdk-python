# AuditEntry

Single audit chain row exposed on the read surface. Mirrors the `internal/audit.Entry` aggregate plus the per-row provenance (`entry_hash`, `prev_hash`) the verifier consumes. `archive_etag` and `archived_at` are populated once the object-store mirror worker has uploaded the row to the per-Domain bucket; both are `null` on rows still in flight from the append path. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**seq** | **int** | Per-Domain monotonic sequence number assigned at append time.  | 
**domain_id** | **UUID** | Owning Domain. | 
**occurred_at** | **datetime** | Server-side timestamp the decision was reached (RFC 3339, UTC).  | 
**reason** | [**AuditReason**](AuditReason.md) |  | 
**subject** | [**AuditSubject**](AuditSubject.md) |  | 
**relation** | **str** | SpiceDB relation label that was checked. | 
**object** | [**AuditObject**](AuditObject.md) |  | 
**decision** | [**AuditDecision**](AuditDecision.md) |  | 
**correlation_id** | **str** | Inbound request correlation id propagated from the transport layer.  | 
**request_context** | [**AuditRequestContext**](AuditRequestContext.md) |  | 
**entry_hash** | **str** | &#x60;sha256(prev_hash || sha256(canonical_bytes))&#x60; for this row, lowercase hex.  | 
**prev_hash** | **str** | &#x60;entry_hash&#x60; of the preceding row, or 64 zero hex characters on the genesis row (&#x60;seq&#x3D;1&#x60;).  | 
**archive_etag** | **str** | Object-store ETag of the per-Domain mirror copy, or &#x60;null&#x60; until the drain worker has uploaded the row.  | [optional] 
**archived_at** | **datetime** | Server-side timestamp the mirror upload completed, or &#x60;null&#x60; while the row is still in flight.  | [optional] 

## Example

```python
from plexsphere.models.audit_entry import AuditEntry

# TODO update the JSON string below
json = "{}"
# create an instance of AuditEntry from a JSON string
audit_entry_instance = AuditEntry.from_json(json)
# print the JSON string representation of the object
print(AuditEntry.to_json())

# convert the object into a dict
audit_entry_dict = audit_entry_instance.to_dict()
# create an instance of AuditEntry from a dict
audit_entry_from_dict = AuditEntry.from_dict(audit_entry_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


