# IdPBindingStatusRequest

Body for PATCH /v1/admin/idp/{id}/status. Only `active` and `deactivated` are supported from this endpoint; transitions such as `degraded` are driven by background probes and cannot be set manually. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**status** | **str** | New binding status. | 

## Example

```python
from plexsphere.models.id_p_binding_status_request import IdPBindingStatusRequest

# TODO update the JSON string below
json = "{}"
# create an instance of IdPBindingStatusRequest from a JSON string
id_p_binding_status_request_instance = IdPBindingStatusRequest.from_json(json)
# print the JSON string representation of the object
print(IdPBindingStatusRequest.to_json())

# convert the object into a dict
id_p_binding_status_request_dict = id_p_binding_status_request_instance.to_dict()
# create an instance of IdPBindingStatusRequest from a dict
id_p_binding_status_request_from_dict = IdPBindingStatusRequest.from_dict(id_p_binding_status_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


