# BuiltinActionParameter

A single parameter declaration inside a `BuiltinAction`'s `parameters` list.  Every field but `name` is descriptive: plexsphere records what the agent reported and validates nothing against it. In particular `type` is not a closed enum and `required` does not gate dispatch — an execution carrying the wrong parameters is refused by the Node that runs it, which is the only place that knows the current truth. Constraining these here would create a second, staler authority. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**name** | **str** | Parameter identifier as the action expects it in a dispatch&#39;s &#x60;parameters&#x60; object. Non-empty after trimming whitespace; a violation surfaces as 422 &#x60;builtin_action_invalid&#x60;.  | 
**type** | **str** | Optional agent-reported type name (e.g. &#x60;string&#x60;, &#x60;int&#x60;, &#x60;duration&#x60;). Free-form and recorded verbatim — see the schema description for why it is not an enum.  | [optional] 
**required** | **bool** | Whether the agent reports the parameter as mandatory. Descriptive only; dispatch is not gated on it.  | [optional] [default to False]
**default** | **str** | Optional default value the agent applies when the parameter is omitted, rendered as the agent reported it.  | [optional] 
**description** | **str** | Optional human-readable summary of the parameter.  | [optional] 

## Example

```python
from plexsphere.models.builtin_action_parameter import BuiltinActionParameter

# TODO update the JSON string below
json = "{}"
# create an instance of BuiltinActionParameter from a JSON string
builtin_action_parameter_instance = BuiltinActionParameter.from_json(json)
# print the JSON string representation of the object
print(BuiltinActionParameter.to_json())

# convert the object into a dict
builtin_action_parameter_dict = builtin_action_parameter_instance.to_dict()
# create an instance of BuiltinActionParameter from a dict
builtin_action_parameter_from_dict = BuiltinActionParameter.from_dict(builtin_action_parameter_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


