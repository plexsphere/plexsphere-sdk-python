# LabelDefinition

Response shape echoing a persisted Label Definition aggregate . 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**id** | **UUID** | Label Definition identifier (UUIDv7). | 
**scope** | **str** | Scope discriminator. Platform Definitions are visible to every tenant; Domain and Project scope restrict the visibility to the respective aggregate.  | 
**scope_id** | **UUID** | Tenancy identifier. Zero UUID when &#x60;scope&#x3D;platform&#x60;; non-zero Domain identifier when &#x60;scope&#x3D;domain&#x60;; non-zero Project identifier when &#x60;scope&#x3D;project&#x60;.  | 
**local_key** | **str** | Unqualified key token (for example &#x60;env&#x60;, &#x60;cost-center&#x60;).  | 
**qualified_key** | **str** | Fully namespaced key (for example &#x60;platform/env&#x60;, &#x60;acme:checkout/owner&#x60;). Unique across the platform.  | 
**value_schema** | [**LabelValueSchema**](LabelValueSchema.md) |  | 
**applicable_kinds** | **List[str]** | Lowercase object-kind whitelist (resource, node, project, domain, workload, network, …). Assignments whose &#x60;object.kind&#x60; is not in this set are rejected with &#x60;scope-mismatch&#x60;.  | 
**on_delete** | **str** | Policy applied when the Definition is deleted while Assignments still exist.  | 
**cardinality** | **int** | Declared per-object cardinality cap for Assignments of this Definition. &#x60;1&#x60; means at most one Assignment per object; &#x60;0&#x60; means unlimited (bounded only by the global 64-per-object ceiling).  | [default to 1]
**immutable** | **bool** | When &#x60;true&#x60;, the value schema is frozen and Assignments of this Definition may not have their value replaced .  | 
**cloud_tag_propagation** | **bool** | When &#x60;true&#x60;, matching cloud tags are synchronised onto cloud resources by the Provisioning Broker.  | 
**description** | **str** | Optional human description (at most 512 chars). | [optional] 
**created_by** | **str** | Provenance of the aggregate. &#x60;system&#x60; is reserved for seed Definitions owned by the platform itself.  | 
**created_at** | **datetime** |  | 
**updated_at** | **datetime** |  | 

## Example

```python
from plexsphere.models.label_definition import LabelDefinition

# TODO update the JSON string below
json = "{}"
# create an instance of LabelDefinition from a JSON string
label_definition_instance = LabelDefinition.from_json(json)
# print the JSON string representation of the object
print(LabelDefinition.to_json())

# convert the object into a dict
label_definition_dict = label_definition_instance.to_dict()
# create an instance of LabelDefinition from a dict
label_definition_from_dict = LabelDefinition.from_dict(label_definition_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


