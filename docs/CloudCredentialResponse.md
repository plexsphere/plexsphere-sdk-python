# CloudCredentialResponse

Metadata-only projection of a Cloud Credential. The shape is shared by `GetCloudCredential`, `ListCloudCredentials`, and `RevokeCloudCredential` so clients only need one binding.  The projection deliberately omits the KV mount, KV path, KV version, and every byte of secret material — the storage location is a storage-internal detail the operator-facing surface has no reason to expose. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**id** | **UUID** | Cloud Credential identifier (UUIDv7). | 
**cloud_id** | **UUID** | Identifier of the owning Cloud — the residency pivot the ReBAC gate authorises against.  | 
**display_name** | **str** | Human-readable Cloud Credential name. | 
**version** | **int** | Monotonic broker-row version. Starts at 1 on issuance and increments on every rotate.  | 
**status** | [**CloudCredentialStatus**](CloudCredentialStatus.md) |  | 
**expires_at** | **datetime** | Wall-clock expiry budget (UTC). | 
**revoked_at** | **datetime** | Revocation timestamp (UTC). &#x60;null&#x60; until the credential is revoked by an operator.  | [optional] 
**expired_at** | **datetime** | Expiry-observed timestamp (UTC). &#x60;null&#x60; until the sweeper marks the credential expired.  | [optional] 
**created_at** | **datetime** | Aggregate creation timestamp (UTC). | 
**updated_at** | **datetime** | Last-modified timestamp (UTC). Bumped by every lifecycle mutator — rotate, revoke, expire.  | 

## Example

```python
from plexsphere.models.cloud_credential_response import CloudCredentialResponse

# TODO update the JSON string below
json = "{}"
# create an instance of CloudCredentialResponse from a JSON string
cloud_credential_response_instance = CloudCredentialResponse.from_json(json)
# print the JSON string representation of the object
print(CloudCredentialResponse.to_json())

# convert the object into a dict
cloud_credential_response_dict = cloud_credential_response_instance.to_dict()
# create an instance of CloudCredentialResponse from a dict
cloud_credential_response_from_dict = CloudCredentialResponse.from_dict(cloud_credential_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


