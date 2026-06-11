# CredentialRotateMaterial

The new secret material the broker writes into OpenBao KV-v2 on rotate. Accepted INBOUND only — no field of this object is ever echoed back on any read surface. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**payload** | **bytes** | Base64-encoded secret bytes written under &#x60;data.payload&#x60; in KV-v2. Empty payload is rejected with &#x60;400 invalid_rotate_material&#x60;.  | 
**ttl_seconds** | **int** | Expiry budget in seconds from which the broker derives the refreshed &#x60;expires_at&#x60;. Must be a positive whole number of seconds (≤ 365 days).  | 
**key_values** | **Dict[str, str]** | Optional flat data map written alongside the payload in KV-v2. Keys and values are caller-owned and never returned.  | [optional] 

## Example

```python
from plexsphere.models.credential_rotate_material import CredentialRotateMaterial

# TODO update the JSON string below
json = "{}"
# create an instance of CredentialRotateMaterial from a JSON string
credential_rotate_material_instance = CredentialRotateMaterial.from_json(json)
# print the JSON string representation of the object
print(CredentialRotateMaterial.to_json())

# convert the object into a dict
credential_rotate_material_dict = credential_rotate_material_instance.to_dict()
# create an instance of CredentialRotateMaterial from a dict
credential_rotate_material_from_dict = CredentialRotateMaterial.from_dict(credential_rotate_material_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


