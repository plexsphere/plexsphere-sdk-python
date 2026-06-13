# CapacityDimension

Closed enum of the six capacity-and-scale axes the platform tracks per Domain. The wire string is the stable identifier operators and automation match on; it surfaces in the Dashboard capacity view, in the platform-audit threshold-crossing entry, and as the `dimension` member of a `capacity_exceeded` Problem. 

## Enum

* `CapacityDimensionNodes` (value: `'nodes'`)

* `CapacityDimensionSSEFanout` (value: `'sse_fanout'`)

* `CapacityDimensionSecretReads` (value: `'secret_reads'`)

* `CapacityDimensionMediatedSessions` (value: `'mediated_sessions'`)

* `CapacityDimensionObservabilityIngest` (value: `'observability_ingest'`)

* `CapacityDimensionActionExecutions` (value: `'action_executions'`)

[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


