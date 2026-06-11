# RevokeReason

Reason a Session was revoked. The first four are operator-driven revocations supplied on the explicit revoke call; `ttl_expired` and `idle_timeout` are set by the background sweeper. 

## Enum

* `RevokeReasonOperatorRequest` (value: `'operator_request'`)

* `RevokeReasonPolicyViolation` (value: `'policy_violation'`)

* `RevokeReasonIdentityCompromise` (value: `'identity_compromise'`)

* `RevokeReasonResourceDecommissioned` (value: `'resource_decommissioned'`)

* `RevokeReasonTtlExpired` (value: `'ttl_expired'`)

* `RevokeReasonIdleTimeout` (value: `'idle_timeout'`)

[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


