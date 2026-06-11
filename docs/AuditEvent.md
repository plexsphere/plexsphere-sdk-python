# AuditEvent

A single normalised audit event in a `POST /v1/nodes/{id}/audit` batch. The batch body is newline-delimited JSON (`application/x-ndjson`), one `AuditEvent` object per line. The closed `source` set mirrors the audit sources the README pins (Linux auditd via AF_AUDIT Netlink, or Kubernetes audit-log tailing). Events are tagged server-side with the originating Node's Domain and Project and routed to Grafana Loki with a separate retention class; the wire body never carries an identity subject or email. `source`, `action`, `outcome`, and `timestamp` are required. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**source** | **str** | The audit source the event was normalised from. A value outside the closed set (&#x60;auditd&#x60;, &#x60;k8s&#x60;) is rejected with &#x60;400 ingest_batch_malformed&#x60;.  | 
**action** | **str** | The audited action (e.g. the syscall name or Kubernetes verb). Required and non-empty.  | 
**outcome** | **str** | The outcome of the audited action (e.g. &#x60;success&#x60; / &#x60;failure&#x60;). Required and non-empty.  | 
**timestamp** | **datetime** | RFC 3339 timestamp at which the event occurred. Required.  | 

## Example

```python
from plexsphere.models.audit_event import AuditEvent

# TODO update the JSON string below
json = "{}"
# create an instance of AuditEvent from a JSON string
audit_event_instance = AuditEvent.from_json(json)
# print the JSON string representation of the object
print(AuditEvent.to_json())

# convert the object into a dict
audit_event_dict = audit_event_instance.to_dict()
# create an instance of AuditEvent from a dict
audit_event_from_dict = AuditEvent.from_dict(audit_event_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


