# AuditEvent

A single normalised audit event in a `POST /v1/nodes/{id}/audit` batch. The batch body is newline-delimited JSON (`application/x-ndjson`), one `AuditEvent` object per line. The closed `source` set mirrors the audit sources the README pins (Linux auditd via AF_AUDIT Netlink, or Kubernetes audit-log tailing) and is closed on those two on purpose — the `source` field below records why an agent-originated event has no representation here and which leg carries it instead. Events are tagged server-side with the originating Node's Domain and Project and routed to Grafana Loki with a separate retention class; the wire body never carries an identity subject or email. `source`, `action`, `outcome`, and `timestamp` are required. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**source** | **str** | The audit source the event was normalised from. A value outside the closed set (&#x60;auditd&#x60;, &#x60;k8s&#x60;) is rejected with &#x60;400 ingest_batch_malformed&#x60;, and because a batch is validated as a whole, one unrepresentable line refuses the entire batch rather than being dropped from it.  The set is closed on the two SYSTEM audit trails deliberately, and stays closed: this leg carries records about what happened on the host and in the cluster, under a retention class and a SIEM route that exist for exactly that material. The agent&#39;s own operational events are not audit records in this sense and have no value here — an agent restart, for one, is already reported to the control plane by the capability manifest the agent PUTs on boot, carrying the binary version and checksum that a &#x60;plexd started&#x60; line would not. Agent telemetry belongs on &#x60;POST /v1/nodes/{id}/logs&#x60;, which is shaped for it.  An agent with no auditd reader and no Kubernetes audit-log tail therefore has nothing to send on this leg, and that is the intended posture rather than a gap to close by widening the enum. Such an agent should omit the batch; it must not reclassify its own events as &#x60;auditd&#x60; to get them accepted, which would corrupt the one property this leg&#39;s consumers rely on — that a record attributed to a system audit trail came from one.  | 
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


