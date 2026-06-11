# CloudCredentialStatus

Derived lifecycle status of a Cloud Credential. `revoked` when `revoked_at` is set; otherwise `expired` when `expired_at` is set or `expires_at` is in the past; otherwise `active`. The status is computed by the read surface from the lifecycle timestamps — it is not a stored column. 

## Enum

* `ACTIVE` (value: `'active'`)

* `EXPIRED` (value: `'expired'`)

* `REVOKED` (value: `'revoked'`)

[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


