# SignInResponse

Response for POST /v1/auth/sign-in. `code_verifier_handle` is an opaque key the server uses to recover the PKCE verifier on the callback; the verifier itself never leaves the backend. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**authorization_url** | **str** | OIDC authorization URL the caller redirects to. | 
**state** | **str** | Opaque state value correlated on the callback. | 
**code_verifier_handle** | **str** | Opaque handle that resolves the PKCE verifier. | 
**nonce** | **str** | OIDC nonce passed into the ID token. | 

## Example

```python
from plexsphere.models.sign_in_response import SignInResponse

# TODO update the JSON string below
json = "{}"
# create an instance of SignInResponse from a JSON string
sign_in_response_instance = SignInResponse.from_json(json)
# print the JSON string representation of the object
print(SignInResponse.to_json())

# convert the object into a dict
sign_in_response_dict = sign_in_response_instance.to_dict()
# create an instance of SignInResponse from a dict
sign_in_response_from_dict = SignInResponse.from_dict(sign_in_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


