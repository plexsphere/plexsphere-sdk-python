# BlueprintParameterDefault

Optional default value applied when a non-required parameter is omitted. Present only when the parameter declares one; its JSON type matches `type`. Absent for required parameters and for optional parameters with no declared default. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------

## Example

```python
from plexsphere.models.blueprint_parameter_default import BlueprintParameterDefault

# TODO update the JSON string below
json = "{}"
# create an instance of BlueprintParameterDefault from a JSON string
blueprint_parameter_default_instance = BlueprintParameterDefault.from_json(json)
# print the JSON string representation of the object
print(BlueprintParameterDefault.to_json())

# convert the object into a dict
blueprint_parameter_default_dict = blueprint_parameter_default_instance.to_dict()
# create an instance of BlueprintParameterDefault from a dict
blueprint_parameter_default_from_dict = BlueprintParameterDefault.from_dict(blueprint_parameter_default_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


