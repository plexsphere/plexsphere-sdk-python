# CloudCredentialRevokeRequest

Body for `POST /v1/cloud-credentials/{id}/revoke`. The `reason` is recorded on the `CloudCredentialRevoked` outbox event so the revocation carries an operator-supplied audit string. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**reason** | **str** | Operator-supplied revocation rationale. Non-empty — whitespace-only is rejected with &#x60;400 invalid_revoke_reason&#x60;.  | 

## Example

```python
from plexsphere.models.cloud_credential_revoke_request import CloudCredentialRevokeRequest

# TODO update the JSON string below
json = "{}"
# create an instance of CloudCredentialRevokeRequest from a JSON string
cloud_credential_revoke_request_instance = CloudCredentialRevokeRequest.from_json(json)
# print the JSON string representation of the object
print(CloudCredentialRevokeRequest.to_json())

# convert the object into a dict
cloud_credential_revoke_request_dict = cloud_credential_revoke_request_instance.to_dict()
# create an instance of CloudCredentialRevokeRequest from a dict
cloud_credential_revoke_request_from_dict = CloudCredentialRevokeRequest.from_dict(cloud_credential_revoke_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


