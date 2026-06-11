# ExecutionStatus

Lifecycle status of an action invocation on a single target Node, and — when projected onto the Execution header — the aggregate status. `pending` is the initial state before the dispatch is acknowledged; `ack` is set when the Node acknowledges receipt; `started` is set when the Node begins executing; `succeeded`, `failed`, `cancelled`, and `timeout` are the terminal states. The legal advances form a state machine (`pending → ack → started → (succeeded | failed | cancelled)`, with `timeout` reachable from any non-terminal state). 

## Enum

* `ExecutionStatusPending` (value: `'pending'`)

* `ExecutionStatusAck` (value: `'ack'`)

* `ExecutionStatusStarted` (value: `'started'`)

* `ExecutionStatusSucceeded` (value: `'succeeded'`)

* `ExecutionStatusFailed` (value: `'failed'`)

* `ExecutionStatusCancelled` (value: `'cancelled'`)

* `ExecutionStatusTimeout` (value: `'timeout'`)

[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


