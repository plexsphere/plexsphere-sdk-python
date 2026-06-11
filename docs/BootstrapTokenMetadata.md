# BootstrapTokenMetadata

Hash-only metadata view of a persisted BootstrapToken aggregate . The plaintext is intentionally absent — it is only ever surfaced by `BootstrapTokenIssueResponse` and that window closes when the issue response is written. `consumed_at` and `revoked_at` are nullable because both transitions are terminal but optional. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**id** | **UUID** | BootstrapToken identifier (UUIDv7). | 
**project_id** | **UUID** | Project the token is scoped to. | 
**kind** | **str** | Redemption surface discriminator.  | 
**env_prefix** | **str** | Environment segment encoded in the plaintext at issuance time.  | 
**issued_at** | **datetime** | Issuance timestamp (UTC). | 
**expires_at** | **datetime** | Absolute expiry timestamp (UTC). | 
**consumed_at** | **datetime** | Redemption timestamp, or null when the token has not yet been redeemed. A non-null value is terminal — a consumed token cannot be redeemed again.  | [optional] 
**revoked_at** | **datetime** | Revocation timestamp, or null when the token is still live. A non-null value is terminal — a revoked token rejects redemption regardless of expiry.  | [optional] 
**issued_by_user_id** | **UUID** | User who issued the token.  | 

## Example

```python
from plexsphere.models.bootstrap_token_metadata import BootstrapTokenMetadata

# TODO update the JSON string below
json = "{}"
# create an instance of BootstrapTokenMetadata from a JSON string
bootstrap_token_metadata_instance = BootstrapTokenMetadata.from_json(json)
# print the JSON string representation of the object
print(BootstrapTokenMetadata.to_json())

# convert the object into a dict
bootstrap_token_metadata_dict = bootstrap_token_metadata_instance.to_dict()
# create an instance of BootstrapTokenMetadata from a dict
bootstrap_token_metadata_from_dict = BootstrapTokenMetadata.from_dict(bootstrap_token_metadata_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


