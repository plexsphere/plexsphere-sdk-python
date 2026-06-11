# AuditObject

Object-side projection on an audit row — the SpiceDB tuple object the ReBAC check addressed. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**type** | **str** | SpiceDB object type (e.g. &#x60;domain&#x60;, &#x60;node&#x60;, &#x60;resource&#x60;). | 
**id** | **str** | Opaque object identifier within &#x60;type&#x60;. | 

## Example

```python
from plexsphere.models.audit_object import AuditObject

# TODO update the JSON string below
json = "{}"
# create an instance of AuditObject from a JSON string
audit_object_instance = AuditObject.from_json(json)
# print the JSON string representation of the object
print(AuditObject.to_json())

# convert the object into a dict
audit_object_dict = audit_object_instance.to_dict()
# create an instance of AuditObject from a dict
audit_object_from_dict = AuditObject.from_dict(audit_object_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


