# BlueprintParameter

A single typed parameter declaration inside a BlueprintVersion's parameter schema. The parameter type is drawn from the closed scalar taxonomy the Crossplane rendering path accepts; object and array types are intentionally not modelled. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**name** | **str** | Parameter name an operator fills in at provisioning time.  | 
**type** | **str** | Closed-set scalar type of the parameter value. Mirrors the Blueprint Catalog&#39;s supported parameter-type taxonomy.  | 
**required** | **bool** | Whether the operator MUST supply a value. A non-required parameter may carry a &#x60;default&#x60;.  | 
**default** | [**BlueprintParameterDefault**](BlueprintParameterDefault.md) |  | [optional] 

## Example

```python
from plexsphere.models.blueprint_parameter import BlueprintParameter

# TODO update the JSON string below
json = "{}"
# create an instance of BlueprintParameter from a JSON string
blueprint_parameter_instance = BlueprintParameter.from_json(json)
# print the JSON string representation of the object
print(BlueprintParameter.to_json())

# convert the object into a dict
blueprint_parameter_dict = blueprint_parameter_instance.to_dict()
# create an instance of BlueprintParameter from a dict
blueprint_parameter_from_dict = BlueprintParameter.from_dict(blueprint_parameter_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


