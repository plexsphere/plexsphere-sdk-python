# ServiceTokenResponse

Response for POST /v1/auth/service/token (and the success body of POST /v1/auth/device-token once the flow completes). 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**access_token** | **str** | Access token suitable for Bearer-authenticated requests. | 
**token_type** | **str** | Always \&quot;Bearer\&quot; for plexsphere-issued tokens. | 
**expires_in** | **int** | Lifetime of the access token in seconds. | 
**scope** | **str** | Space-delimited scope string, when one was granted. | [optional] 

## Example

```python
from plexsphere.models.service_token_response import ServiceTokenResponse

# TODO update the JSON string below
json = "{}"
# create an instance of ServiceTokenResponse from a JSON string
service_token_response_instance = ServiceTokenResponse.from_json(json)
# print the JSON string representation of the object
print(ServiceTokenResponse.to_json())

# convert the object into a dict
service_token_response_dict = service_token_response_instance.to_dict()
# create an instance of ServiceTokenResponse from a dict
service_token_response_from_dict = ServiceTokenResponse.from_dict(service_token_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


