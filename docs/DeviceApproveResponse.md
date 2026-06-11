# DeviceApproveResponse

Response for POST /v1/auth/device/approve. The body is intentionally minimal — the polling client picks up the minted token via /v1/auth/device-token; this response just confirms the status flip so the dashboard can transition into a \"you can close this tab\" affordance. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**status** | **str** | Always &#x60;approved&#x60; on a 200. | 

## Example

```python
from plexsphere.models.device_approve_response import DeviceApproveResponse

# TODO update the JSON string below
json = "{}"
# create an instance of DeviceApproveResponse from a JSON string
device_approve_response_instance = DeviceApproveResponse.from_json(json)
# print the JSON string representation of the object
print(DeviceApproveResponse.to_json())

# convert the object into a dict
device_approve_response_dict = device_approve_response_instance.to_dict()
# create an instance of DeviceApproveResponse from a dict
device_approve_response_from_dict = DeviceApproveResponse.from_dict(device_approve_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


