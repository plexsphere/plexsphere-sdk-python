# APITokenRotateResponse

Response for POST /v1/auth/tokens/{id}/rotate. The `Sunset` header names the retirement deadline of the rotated-from plaintext. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**id** | **UUID** | Token identifier (UUIDv7). | 
**plaintext** | **str** | New psk-shaped plaintext; returned exactly once. | 
**env_prefix** | **str** | Environment segment embedded in the plaintext. | 
**expires_at** | **datetime** | Expiry timestamp of the new plaintext. | 
**rotation_deadline** | **datetime** | Retirement deadline of the rotated-from plaintext. Matches the value advertised in the &#x60;Sunset&#x60; response header.  | 

## Example

```python
from plexsphere.models.api_token_rotate_response import APITokenRotateResponse

# TODO update the JSON string below
json = "{}"
# create an instance of APITokenRotateResponse from a JSON string
api_token_rotate_response_instance = APITokenRotateResponse.from_json(json)
# print the JSON string representation of the object
print(APITokenRotateResponse.to_json())

# convert the object into a dict
api_token_rotate_response_dict = api_token_rotate_response_instance.to_dict()
# create an instance of APITokenRotateResponse from a dict
api_token_rotate_response_from_dict = APITokenRotateResponse.from_dict(api_token_rotate_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


