# BuiltinAction

A single built-in action inside a `CapabilityManifestRequest`'s `builtin_actions` inventory: what the agent calls it, what it does, and what it takes.  `name` is the value a dispatch puts in `DispatchExecutionRequest. action` alongside `kind: builtin`. Its constraint deliberately matches that field's rather than being tightened here — a name the inventory could not express would be an action the Node can run and the platform could not describe. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**name** | **str** | Action identifier (e.g. &#x60;diagnostics.collect&#x60;, &#x60;service.upgrade&#x60;). Non-empty after trimming whitespace; a violation surfaces as 422 &#x60;builtin_action_invalid&#x60;. Unique within one manifest — a duplicate surfaces as 422 &#x60;builtin_action_duplicate&#x60;.  | 
**description** | **str** | Optional human-readable summary of what the action does, shown on operator surfaces beside the action name.  | [optional] 
**parameters** | [**List[BuiltinActionParameter]**](BuiltinActionParameter.md) | Optional list of the parameters the action accepts. Entries are ordered as the agent reported them, which is the order an operator surface should present them in. Duplicate parameter names within one action surface as 422 &#x60;builtin_action_invalid&#x60;.  | [optional] 

## Example

```python
from plexsphere.models.builtin_action import BuiltinAction

# TODO update the JSON string below
json = "{}"
# create an instance of BuiltinAction from a JSON string
builtin_action_instance = BuiltinAction.from_json(json)
# print the JSON string representation of the object
print(BuiltinAction.to_json())

# convert the object into a dict
builtin_action_dict = builtin_action_instance.to_dict()
# create an instance of BuiltinAction from a dict
builtin_action_from_dict = BuiltinAction.from_dict(builtin_action_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


