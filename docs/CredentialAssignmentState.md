# CredentialAssignmentState

Lifecycle state of a Credential Assignment. `requested` is the opening state; `approved` materialises the binding; `rejected` closes an unapproved request; `revoked` tears down a previously approved binding. The state is a stored column advanced only by the application service's transition rules. 

## Enum

* `REQUESTED` (value: `'requested'`)

* `APPROVED` (value: `'approved'`)

* `REJECTED` (value: `'rejected'`)

* `REVOKED` (value: `'revoked'`)

[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


