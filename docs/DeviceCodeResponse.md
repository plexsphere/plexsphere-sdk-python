# DeviceCodeResponse

Response for POST /v1/auth/device-code. Fields mirror RFC 8628 §3.2. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**device_code** | **str** | Opaque code the client polls with. | 
**user_code** | **str** | Short code the end user types in a browser. | 
**verification_uri** | **str** | URL the end user visits to confirm the device. | 
**verification_uri_complete** | **str** | URL with the user_code pre-filled; RFC 8628 recommends presenting this as a QR code.  | 
**expires_in** | **int** | Lifetime of the device_code in seconds. | 
**interval** | **int** | Minimum polling interval in seconds. | 

## Example

```python
from plexsphere.models.device_code_response import DeviceCodeResponse

# TODO update the JSON string below
json = "{}"
# create an instance of DeviceCodeResponse from a JSON string
device_code_response_instance = DeviceCodeResponse.from_json(json)
# print the JSON string representation of the object
print(DeviceCodeResponse.to_json())

# convert the object into a dict
device_code_response_dict = device_code_response_instance.to_dict()
# create an instance of DeviceCodeResponse from a dict
device_code_response_from_dict = DeviceCodeResponse.from_dict(device_code_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


