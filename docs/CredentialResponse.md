# CredentialResponse

Metadata-only projection of an OpenBao-brokered credential. The shape is shared by `GetCredential`, `ListProjectCredentials`, `RevokeCredential`, and `RotateCredential` so clients only need one binding.  The projection deliberately omits the KV mount, KV path, KV version, and every byte of secret material — the storage location is a storage-internal detail the operator-facing surface has no reason to expose. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**id** | **UUID** | Credential identifier (UUIDv7). | 
**project_id** | **UUID** | Identifier of the owning Project — the residency pivot the ReBAC gate authorises against.  | 
**version** | **int** | Monotonic broker-row version. Starts at 1 on issuance and increments on every rotate.  | 
**status** | [**CloudCredentialStatus**](CloudCredentialStatus.md) | Derived lifecycle status of the credential. &#x60;revoked&#x60; when &#x60;revoked_at&#x60; is set; otherwise &#x60;expired&#x60; when &#x60;expired_at&#x60; is set or &#x60;expires_at&#x60; is in the past; otherwise &#x60;active&#x60;. Computed by the read surface from the lifecycle timestamps — it is not a stored column. Reuses the shared &#x60;CloudCredentialStatus&#x60; closed enum (identical active/expired/revoked vocabulary and precedence) rather than declaring a byte-identical parallel enum.  | 
**expires_at** | **datetime** | Wall-clock expiry budget (UTC). | 
**revoked_at** | **datetime** | Revocation timestamp (UTC). &#x60;null&#x60; until the credential is revoked by an operator.  | [optional] 
**expired_at** | **datetime** | Expiry-observed timestamp (UTC). &#x60;null&#x60; until the sweeper marks the credential expired.  | [optional] 
**created_at** | **datetime** | Aggregate creation timestamp (UTC). | 
**updated_at** | **datetime** | Last-modified timestamp (UTC). Bumped by every lifecycle mutator — rotate, revoke, expire.  | 

## Example

```python
from plexsphere.models.credential_response import CredentialResponse

# TODO update the JSON string below
json = "{}"
# create an instance of CredentialResponse from a JSON string
credential_response_instance = CredentialResponse.from_json(json)
# print the JSON string representation of the object
print(CredentialResponse.to_json())

# convert the object into a dict
credential_response_dict = credential_response_instance.to_dict()
# create an instance of CredentialResponse from a dict
credential_response_from_dict = CredentialResponse.from_dict(credential_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


