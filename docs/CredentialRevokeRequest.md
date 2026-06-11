# CredentialRevokeRequest

Body for `POST /v1/credentials/{id}/revoke`. The `reason` is recorded on the `CredentialRevoked` outbox event so the revocation carries an operator-supplied audit string. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**reason** | **str** | Operator-supplied revocation rationale. Non-empty — whitespace-only is rejected with &#x60;400 invalid_revoke_reason&#x60;.  | 

## Example

```python
from plexsphere.models.credential_revoke_request import CredentialRevokeRequest

# TODO update the JSON string below
json = "{}"
# create an instance of CredentialRevokeRequest from a JSON string
credential_revoke_request_instance = CredentialRevokeRequest.from_json(json)
# print the JSON string representation of the object
print(CredentialRevokeRequest.to_json())

# convert the object into a dict
credential_revoke_request_dict = credential_revoke_request_instance.to_dict()
# create an instance of CredentialRevokeRequest from a dict
credential_revoke_request_from_dict = CredentialRevokeRequest.from_dict(credential_revoke_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


