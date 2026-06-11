# LabelDefinitionRequest

Body for POST /v1/label-definitions. Mirrors the DDD `CreateDefinitionParams` shape: the caller names the scope as a discriminator + optional UUID, the local key, and the value contract. The services layer performs the `manage` ReBAC check on the scope-object before persisting. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**scope** | **str** | Scope discriminator. | 
**scope_id** | **UUID** | Tenancy identifier. Required when &#x60;scope&#x60; is &#x60;domain&#x60; or &#x60;project&#x60;; omitted when &#x60;scope&#x3D;platform&#x60;.  | [optional] 
**local_key** | **str** | Unqualified key token — lowercase letters, digits, &#x60;.&#x60;, &#x60;_&#x60;, &#x60;-&#x60;; 1–64 characters; no leading or trailing separator.  | 
**description** | **str** |  | [optional] 
**value_schema** | [**LabelValueSchema**](LabelValueSchema.md) |  | 
**applicable_kinds** | **List[str]** |  | 
**required** | **bool** | When &#x60;true&#x60;, Assignments of this Definition are mandatory on every object of the applicable kinds.  | [optional] 
**default_value** | **object** |  | [optional] 
**immutable** | **bool** |  | [optional] 
**cloud_tag_propagation** | **bool** |  | [optional] 
**on_delete** | **str** | On-delete policy. Defaults to &#x60;block&#x60; when omitted .  | [optional] 
**cardinality** | **int** | Declared per-object cardinality cap for Assignments of this Definition. Defaults to &#x60;1&#x60; when omitted .  | [optional] 

## Example

```python
from plexsphere.models.label_definition_request import LabelDefinitionRequest

# TODO update the JSON string below
json = "{}"
# create an instance of LabelDefinitionRequest from a JSON string
label_definition_request_instance = LabelDefinitionRequest.from_json(json)
# print the JSON string representation of the object
print(LabelDefinitionRequest.to_json())

# convert the object into a dict
label_definition_request_dict = label_definition_request_instance.to_dict()
# create an instance of LabelDefinitionRequest from a dict
label_definition_request_from_dict = LabelDefinitionRequest.from_dict(label_definition_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


