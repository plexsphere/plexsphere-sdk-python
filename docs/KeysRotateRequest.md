# KeysRotateRequest

Body for POST /v1/keys/rotate. Carries the single artifact a rotating plexd Node submits to complete a mesh-key rotation: the freshly-generated Curve25519 public key. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**new_public_key** | **bytes** | The Node&#39;s freshly-generated Curve25519 (WireGuard) public key, 32 bytes, base64-encoded with standard padding. RFC 7748 §5 fixes the curve to a 32-byte little-endian point encoding, so any other decoded length is rejected with 422 &#x60;keys_rotate_public_key_invalid&#x60;; the all-zero degenerate value (a known small-order point) is rejected with the same code. A key byte-identical to the Node&#39;s current public key is rejected with 422 &#x60;keys_rotate_public_key_unchanged&#x60;.  | 

## Example

```python
from plexsphere.models.keys_rotate_request import KeysRotateRequest

# TODO update the JSON string below
json = "{}"
# create an instance of KeysRotateRequest from a JSON string
keys_rotate_request_instance = KeysRotateRequest.from_json(json)
# print the JSON string representation of the object
print(KeysRotateRequest.to_json())

# convert the object into a dict
keys_rotate_request_dict = keys_rotate_request_instance.to_dict()
# create an instance of KeysRotateRequest from a dict
keys_rotate_request_from_dict = KeysRotateRequest.from_dict(keys_rotate_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


