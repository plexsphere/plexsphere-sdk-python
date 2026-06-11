# PolicySelector

Source / destination selector pair scoping the Policy to a subset of the Project's Nodes. The selector grammar is the same label-selector grammar exposed at `POST /v1/labels/selectors/preview` — `key=value`, `key in (a, b)`, `key`, `!key`, comma-separated AND. Empty source or destination rejects at the aggregate boundary with `422 selector_syntax_error`. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**source** | **str** | Raw label-selector expression matching the originating side of governed traffic (e.g. &#x60;env&#x3D;prod, role in (api, web)&#x60;).  | 
**destination** | **str** | Raw label-selector expression matching the terminating side of governed traffic.  | 

## Example

```python
from plexsphere.models.policy_selector import PolicySelector

# TODO update the JSON string below
json = "{}"
# create an instance of PolicySelector from a JSON string
policy_selector_instance = PolicySelector.from_json(json)
# print the JSON string representation of the object
print(PolicySelector.to_json())

# convert the object into a dict
policy_selector_dict = policy_selector_instance.to_dict()
# create an instance of PolicySelector from a dict
policy_selector_from_dict = PolicySelector.from_dict(policy_selector_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


