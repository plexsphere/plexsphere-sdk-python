# ApprovalState

Lifecycle state of an Approval. `proposed` is the opening state before the policy is evaluated; `pending-approval` means the Domain's `ApprovalPolicy` gated the proposal and it awaits an operator decision; `approved` is the terminal accept state (reached by an operator decision, the empty-policy short-circuit, or a break-glass override); `rejected` is the terminal state an operator decision declines into; `expired` is the terminal state the background sweeper drives an un-decided proposal into past its deadline. The state is a stored column advanced only by the application service's transition rules. 

## Enum

* `PROPOSED` (value: `'proposed'`)

* `PENDING_MINUS_APPROVAL` (value: `'pending-approval'`)

* `APPROVED` (value: `'approved'`)

* `REJECTED` (value: `'rejected'`)

* `EXPIRED` (value: `'expired'`)

[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


