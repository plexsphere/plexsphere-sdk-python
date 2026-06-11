# CredentialRotateRequest

Body for `POST /v1/credentials/{id}/rotate`. `expected_version` is the broker-row version the caller observed via a prior read; a mismatch surfaces as `409 credential_cas_conflict` so concurrent rotations fail closed. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**expected_version** | **int** | The broker-row &#x60;version&#x60; the caller observed. The Custodian performs a compare-and-swap against this value.  | 
**material** | [**CredentialRotateMaterial**](CredentialRotateMaterial.md) |  | 

## Example

```python
from plexsphere.models.credential_rotate_request import CredentialRotateRequest

# TODO update the JSON string below
json = "{}"
# create an instance of CredentialRotateRequest from a JSON string
credential_rotate_request_instance = CredentialRotateRequest.from_json(json)
# print the JSON string representation of the object
print(CredentialRotateRequest.to_json())

# convert the object into a dict
credential_rotate_request_dict = credential_rotate_request_instance.to_dict()
# create an instance of CredentialRotateRequest from a dict
credential_rotate_request_from_dict = CredentialRotateRequest.from_dict(credential_rotate_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


