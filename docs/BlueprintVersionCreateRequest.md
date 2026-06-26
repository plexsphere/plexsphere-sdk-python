# BlueprintVersionCreateRequest

Body for `POST /v1/blueprints/{id}/versions`. The service validates every field BEFORE persisting: the `provider_kinds` enum members, the `injection_strategy` enum, the `parameter_schema` document, and the structural XRD/Composition manifest pair.  The `parameter_schema` here is the canonical `{\"parameters\":[...]}` document the Blueprint Catalog stores — NOT the flattened array the `BlueprintVersionResponse` read surface projects. Authorship and read shapes differ on purpose: authors submit the document the aggregate parses, readers receive the projected parameter list. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**version** | **str** | Version identifier, unique within the parent Blueprint. | 
**xrd** | **Dict[str, object]** | Crossplane CompositeResourceDefinition manifest as a JSON object. Validated structurally against the Composition before any write; a malformed pair surfaces as &#x60;400 invalid_manifest&#x60;.  | 
**composition** | **Dict[str, object]** | Crossplane Composition manifest as a JSON object. Its composite-type reference must name the composite type the XRD declares.  | 
**parameter_schema** | **Dict[str, object]** | Typed parameter-schema document of the form &#x60;{\&quot;parameters\&quot;:[{\&quot;name\&quot;:…,\&quot;type\&quot;:…,\&quot;required\&quot;:…,\&quot;default\&quot;?:…}]}&#x60;. A structurally invalid document surfaces as &#x60;400 invalid_parameter_schema&#x60;.  | 
**provider_kinds** | **List[str]** | Closed-set infrastructure substrates this version can target, one or more of &#x60;aws&#x60;, &#x60;gcp&#x60;, &#x60;hetzner&#x60;, &#x60;openstack&#x60;. The server validates each value through the domain provider-kind parser; an out-of-set value is rejected with &#x60;400 invalid_provider_kind&#x60;. Non-empty.  | 
**injection_strategy** | **str** | Discriminator naming how this version threads request parameters into the rendered Composite Resource, one of &#x60;cloud-init-user-data&#x60;, &#x60;helm-values&#x60;, &#x60;provider-secret&#x60;. The server validates the value; an out-of-set value is rejected with &#x60;400 invalid_injection_strategy&#x60;.  | 

## Example

```python
from plexsphere.models.blueprint_version_create_request import BlueprintVersionCreateRequest

# TODO update the JSON string below
json = "{}"
# create an instance of BlueprintVersionCreateRequest from a JSON string
blueprint_version_create_request_instance = BlueprintVersionCreateRequest.from_json(json)
# print the JSON string representation of the object
print(BlueprintVersionCreateRequest.to_json())

# convert the object into a dict
blueprint_version_create_request_dict = blueprint_version_create_request_instance.to_dict()
# create an instance of BlueprintVersionCreateRequest from a dict
blueprint_version_create_request_from_dict = BlueprintVersionCreateRequest.from_dict(blueprint_version_create_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


