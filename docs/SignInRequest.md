# SignInRequest

Body for POST /v1/auth/sign-in. At least one of `domain_id` or `idp_binding_id` must be supplied; the router uses them to resolve the IdP binding against which to begin the flow . 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**domain_id** | **UUID** | Domain the user is signing into. | [optional] 
**idp_binding_id** | **UUID** | Explicit IdP binding override within a Domain. | [optional] 
**return_to** | **str** | Post-sign-in redirect target. Must be an allow-listed relative path or absolute URL.  | [optional] 
**prompt** | **str** | Optional OIDC &#x60;prompt&#x60; parameter forwarded to the IdP (e.g. \&quot;login\&quot;, \&quot;consent\&quot;).  | [optional] 

## Example

```python
from plexsphere.models.sign_in_request import SignInRequest

# TODO update the JSON string below
json = "{}"
# create an instance of SignInRequest from a JSON string
sign_in_request_instance = SignInRequest.from_json(json)
# print the JSON string representation of the object
print(SignInRequest.to_json())

# convert the object into a dict
sign_in_request_dict = sign_in_request_instance.to_dict()
# create an instance of SignInRequest from a dict
sign_in_request_from_dict = SignInRequest.from_dict(sign_in_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


