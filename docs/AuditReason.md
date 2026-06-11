# AuditReason

Frozen ReBAC decision-reason taxonomy attached to every audit chain row. The value set mirrors `internal/audit.Reason.String` exactly: any new reason is a breaking change and MUST be appended to this enum in lockstep with the Go side. The `unknown` sentinel is the renderer fallback for an out-of-range ordinal — its presence on the wire is itself an audit anomaly worth alerting on. 

## Enum

* `GRANTED` (value: `'granted'`)

* `OUT_OF_SCOPE` (value: `'out_of_scope'`)

* `INSUFFICIENT_RELATION` (value: `'insufficient_relation'`)

* `CAVEAT_VIOLATION` (value: `'caveat_violation'`)

* `UNKNOWN` (value: `'unknown'`)

[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


