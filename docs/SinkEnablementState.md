# SinkEnablementState

Lifecycle state of a sink enablement. `requested` is the opening state a request is filed in; `approved` is the state that makes the sink usable in the Project; `rejected` is terminal and is reached when a decider declines a request; `revoked` is terminal and is reached when an approved grant is withdrawn. The legal transitions are requested → approved, requested → rejected, and approved → revoked. 

## Enum

* `REQUESTED` (value: `'requested'`)

* `APPROVED` (value: `'approved'`)

* `REJECTED` (value: `'rejected'`)

* `REVOKED` (value: `'revoked'`)

[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


