# ResourceResponse

Hydrated projection of a Tenancy Resource. The shape is shared by `CreateResource`, `GetResource`, and `ListProjectResources` — every read surface returns the same wire bytes for the same Resource so clients only need one binding. The `provisioning` sub-object is present only on `Origin=Provisioned` Resources. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**id** | **UUID** | Resource identifier (UUIDv7). | 
**project_id** | **UUID** | Owning Project (UUIDv7) — exactly-one-parent rule.  | 
**domain_id** | **UUID** | Owning Domain (UUIDv7). Denormalised from the parent Project so cross-Domain checks do not have to reload the Project on every Resource read.  | 
**kind** | **str** | Resource kind discriminator. | 
**external_ref** | **str** | Optional external-system reference. &#x60;null&#x60; when the Resource declared none.  | [optional] 
**origin** | [**ResourceOrigin**](ResourceOrigin.md) |  | 
**created_at** | **datetime** | Resource creation timestamp (UTC). | 
**updated_at** | **datetime** | Last-modified timestamp (UTC). | 
**provisioning** | [**ResourceProvisioning**](ResourceProvisioning.md) | Broker provisioning state. Present only on &#x60;Origin&#x3D;Provisioned&#x60; Resources; omitted on &#x60;Origin&#x3D;Adopted&#x60; Resources.  | [optional] 

## Example

```python
from plexsphere.models.resource_response import ResourceResponse

# TODO update the JSON string below
json = "{}"
# create an instance of ResourceResponse from a JSON string
resource_response_instance = ResourceResponse.from_json(json)
# print the JSON string representation of the object
print(ResourceResponse.to_json())

# convert the object into a dict
resource_response_dict = resource_response_instance.to_dict()
# create an instance of ResourceResponse from a dict
resource_response_from_dict = ResourceResponse.from_dict(resource_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


