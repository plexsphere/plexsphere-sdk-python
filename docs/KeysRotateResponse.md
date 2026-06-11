# KeysRotateResponse

Body for POST /v1/keys/rotate success responses. Carries the receipt of the completed rotation: the `peer_key_rotation` row id and the `(kid, wrap_key_version)` reference of the re-issued pairwise PSK.  The response deliberately carries NO PSK plaintext and NO ciphertext — only the `(kid, wrap_key_version)` reference. The rotating Node already holds its NSK and resolves the PSK wrapping locally; the control plane never re-emits secret material on this surface. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**rotation_id** | **UUID** | Identifier of the &#x60;peer_key_rotation&#x60; row this submission flipped from &#x60;pending&#x60; to &#x60;completed&#x60;. The Node records it so a later audit query can correlate the rotation back to the re-issued PSK row.  | 
**kid** | **str** | Key id of the freshly-wrapped pairwise PSK the rotation re-issued. Paired with &#x60;wrap_key_version&#x60; it is the reference plexd uses to resolve the PSK without the control plane re-transmitting secret material.  | 
**wrap_key_version** | **int** | Version of the active wrap key under which the re-issued PSK was wrapped. Paired with &#x60;kid&#x60; it pins the exact wrapping epoch.  | 

## Example

```python
from plexsphere.models.keys_rotate_response import KeysRotateResponse

# TODO update the JSON string below
json = "{}"
# create an instance of KeysRotateResponse from a JSON string
keys_rotate_response_instance = KeysRotateResponse.from_json(json)
# print the JSON string representation of the object
print(KeysRotateResponse.to_json())

# convert the object into a dict
keys_rotate_response_dict = keys_rotate_response_instance.to_dict()
# create an instance of KeysRotateResponse from a dict
keys_rotate_response_from_dict = KeysRotateResponse.from_dict(keys_rotate_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


