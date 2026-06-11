# ResourceProvisioning

Broker provisioning state attached to an `Origin=Provisioned` Resource. Absent on `Origin=Adopted` Resources. The phase is the reconcile loop's most recently observed snapshot and is guaranteed to advance only toward a terminal. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**phase** | [**ProvisionedResourcePhase**](ProvisionedResourcePhase.md) |  | 
**provisioned_resource_id** | **UUID** | Identifier of the broker-owned ProvisionedResource bound to this Resource (UUIDv7).  | 

## Example

```python
from plexsphere.models.resource_provisioning import ResourceProvisioning

# TODO update the JSON string below
json = "{}"
# create an instance of ResourceProvisioning from a JSON string
resource_provisioning_instance = ResourceProvisioning.from_json(json)
# print the JSON string representation of the object
print(ResourceProvisioning.to_json())

# convert the object into a dict
resource_provisioning_dict = resource_provisioning_instance.to_dict()
# create an instance of ResourceProvisioning from a dict
resource_provisioning_from_dict = ResourceProvisioning.from_dict(resource_provisioning_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


