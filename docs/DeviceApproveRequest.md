# DeviceApproveRequest

Body for POST /v1/auth/device/approve. The dashboard reads `user_code` from the URL the CLI printed (`?user_code=ABCD-EFGH`) and round-trips it back to the server verbatim — the server normalises by stripping the optional ASCII dash before comparing against the `user_code` produced by `POST /v1/auth/device-code`. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**user_code** | **str** | Short user-typed code from the device-code response. The base32 alphabet is uppercase A–Z and 2–7; an optional ASCII dash separating the two 4-character halves (e.g. &#x60;ABCD-EFGH&#x60; or &#x60;ABCDEFGH&#x60;) is accepted.  | 

## Example

```python
from plexsphere.models.device_approve_request import DeviceApproveRequest

# TODO update the JSON string below
json = "{}"
# create an instance of DeviceApproveRequest from a JSON string
device_approve_request_instance = DeviceApproveRequest.from_json(json)
# print the JSON string representation of the object
print(DeviceApproveRequest.to_json())

# convert the object into a dict
device_approve_request_dict = device_approve_request_instance.to_dict()
# create an instance of DeviceApproveRequest from a dict
device_approve_request_from_dict = DeviceApproveRequest.from_dict(device_approve_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


