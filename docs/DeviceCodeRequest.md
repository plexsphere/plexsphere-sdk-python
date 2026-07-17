# DeviceCodeRequest

Body for POST /v1/auth/device-code — initiates RFC 8628 device authorization against the resolved IdP binding. For a tenant login `domain_id` selects the Domain and the router resolves the binding from it when `idp_binding_id` is omitted. Omitting `domain_id` starts a **platform-operator** device authorization when the named binding (via `idp_binding_id` or `idp_binding_alias`) is a platform-shared binding — mirroring the browser sign-in contract; any non-platform resolution without a `domain_id` is rejected 400. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**domain_id** | **UUID** | Domain the caller is authenticating against. Optional: omitting it together with a platform-shared binding named via &#x60;idp_binding_id&#x60; or &#x60;idp_binding_alias&#x60; starts a Domain-independent platform-operator device authorization. A per-Domain binding named without a &#x60;domain_id&#x60; is rejected 400 on this surface.  | [optional] 
**idp_binding_id** | **UUID** | Explicit IdP binding within the Domain. Optional: when omitted, the binding is resolved by alias, then the Domain&#39;s primary binding, then its single active binding. A Domain with two or more active bindings and no primary requires this field (or idp_binding_alias) to disambiguate. With no &#x60;domain_id&#x60;, must name a platform-shared binding to start a platform-operator device authorization.  | [optional] 
**idp_binding_alias** | **str** | Human-friendly alias of an IdP binding within the Domain (e.g. &#x60;github&#x60;). Optional. Resolution precedence is explicit id, then alias, then the Domain&#39;s primary binding, then its single active binding. Mutually exclusive with idp_binding_id. With no &#x60;domain_id&#x60;, must name a platform-shared binding&#39;s alias (globally unique among active platform bindings) to start a platform-operator device authorization.  | [optional] 
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


