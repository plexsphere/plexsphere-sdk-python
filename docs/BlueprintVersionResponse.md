# BlueprintVersionResponse

A single version of a Blueprint. A version is keyed by its parent Blueprint and its `version` string and carries the typed `parameter_schema` an operator fills in when provisioning a Resource. The Crossplane XRD and Composition manifests are storage-internal and are deliberately not exposed on this read surface. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**id** | **UUID** | Surrogate identifier of this immutable version (UUIDv7) — the handle a Resource references as &#x60;blueprint_version_id&#x60; when it is provisioned. The version is keyed for humans by its &#x60;version&#x60; string within the parent Blueprint; this id is the stable machine handle &#x60;resource create&#x60; consumes.  | 
**version** | **str** | Version identifier, unique within the parent Blueprint. | 
**provider_kinds** | **List[str]** | Closed-set infrastructure substrates this version can target. Non-empty.  | 
**injection_strategy** | **str** | Closed-set discriminator naming how this version threads request parameters into the rendered Composite Resource.  | 
**parameter_schema** | [**List[BlueprintParameter]**](BlueprintParameter.md) | Typed parameter declarations an operator fills in when provisioning a Resource from this version. May be empty when the version declares no parameters.  | 
**created_at** | **datetime** | Version creation timestamp (UTC). | 

## Example

```python
from plexsphere.models.blueprint_version_response import BlueprintVersionResponse

# TODO update the JSON string below
json = "{}"
# create an instance of BlueprintVersionResponse from a JSON string
blueprint_version_response_instance = BlueprintVersionResponse.from_json(json)
# print the JSON string representation of the object
print(BlueprintVersionResponse.to_json())

# convert the object into a dict
blueprint_version_response_dict = blueprint_version_response_instance.to_dict()
# create an instance of BlueprintVersionResponse from a dict
blueprint_version_response_from_dict = BlueprintVersionResponse.from_dict(blueprint_version_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


