# ServiceTokenRequest

Body for POST /v1/auth/service/token. Supports `client_credentials` and JWT-bearer grant types. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**grant_type** | **str** | OAuth2 grant type used to authenticate the service. | 
**client_id** | **str** | ServiceIdentity client identifier. | [optional] 
**client_secret** | **str** | Client secret for the &#x60;client_credentials&#x60; grant. Rejected for the JWT-bearer grant.  | [optional] 
**client_assertion** | **str** | Signed JWT assertion for the JWT-bearer grant. Rejected for the &#x60;client_credentials&#x60; grant.  | [optional] 
**scope** | **str** | Optional space-delimited OAuth2 scope list. | [optional] 

## Example

```python
from plexsphere.models.service_token_request import ServiceTokenRequest

# TODO update the JSON string below
json = "{}"
# create an instance of ServiceTokenRequest from a JSON string
service_token_request_instance = ServiceTokenRequest.from_json(json)
# print the JSON string representation of the object
print(ServiceTokenRequest.to_json())

# convert the object into a dict
service_token_request_dict = service_token_request_instance.to_dict()
# create an instance of ServiceTokenRequest from a dict
service_token_request_from_dict = ServiceTokenRequest.from_dict(service_token_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


