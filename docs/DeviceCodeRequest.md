# DeviceCodeRequest

Body for POST /v1/auth/device-code — initiates RFC 8628 device authorization against the resolved IdP binding. Only `domain_id` is required; the router resolves the binding from the Domain when `idp_binding_id` is omitted. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**domain_id** | **UUID** | Domain the caller is authenticating against. | 
**idp_binding_id** | **UUID** | Explicit IdP binding within the Domain. Optional: when omitted, the binding is resolved by alias, then the Domain&#39;s primary binding, then its single active binding. A Domain with two or more active bindings and no primary requires this field (or idp_binding_alias) to disambiguate.  | [optional] 
**idp_binding_alias** | **str** | Human-friendly alias of an IdP binding within the Domain (e.g. &#x60;github&#x60;). Optional. Resolution precedence is explicit id, then alias, then the Domain&#39;s primary binding, then its single active binding. Mutually exclusive with idp_binding_id.  | [optional] 
**client_id** | **str** | Optional OIDC client identifier override. | [optional] 

## Example

```python
from plexsphere.models.device_code_request import DeviceCodeRequest

# TODO update the JSON string below
json = "{}"
# create an instance of DeviceCodeRequest from a JSON string
device_code_request_instance = DeviceCodeRequest.from_json(json)
# print the JSON string representation of the object
print(DeviceCodeRequest.to_json())

# convert the object into a dict
device_code_request_dict = device_code_request_instance.to_dict()
# create an instance of DeviceCodeRequest from a dict
device_code_request_from_dict = DeviceCodeRequest.from_dict(device_code_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


