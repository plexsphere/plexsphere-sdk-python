# CloudCredentialAttachRequest

Body for `POST /v1/cloud-credentials/{id}/clouds`. Names the usage Cloud to attach so the credential additionally serves it. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**cloud_id** | **UUID** | Identifier of the usage Cloud to attach. Must be a non-zero UUID — a malformed value is rejected with &#x60;400 invalid_cloud_id&#x60;.  | 

## Example

```python
from plexsphere.models.cloud_credential_attach_request import CloudCredentialAttachRequest

# TODO update the JSON string below
json = "{}"
# create an instance of CloudCredentialAttachRequest from a JSON string
cloud_credential_attach_request_instance = CloudCredentialAttachRequest.from_json(json)
# print the JSON string representation of the object
print(CloudCredentialAttachRequest.to_json())

# convert the object into a dict
cloud_credential_attach_request_dict = cloud_credential_attach_request_instance.to_dict()
# create an instance of CloudCredentialAttachRequest from a dict
cloud_credential_attach_request_from_dict = CloudCredentialAttachRequest.from_dict(cloud_credential_attach_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


