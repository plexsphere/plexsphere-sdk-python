# LabelSelectorASTNode

Parsed selector AST node. The `op` discriminator names the clause kind; sibling fields carry the clause payload. AND composition nests via `children` so a consumer can walk the tree without a second schema. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**op** | **str** | Clause discriminator. | 
**key** | **str** | Label key the clause targets. Present for &#x60;eq&#x60;, &#x60;neq&#x60;, &#x60;in&#x60;, &#x60;exists&#x60;, &#x60;absent&#x60;. Absent for &#x60;and&#x60;.  | [optional] 
**value** | **str** | Value literal for &#x60;eq&#x60; and &#x60;neq&#x60;. | [optional] 
**values** | **List[str]** | Value set for &#x60;in&#x60;. | [optional] 
**children** | [**List[LabelSelectorASTNode]**](LabelSelectorASTNode.md) | AND children. | [optional] 

## Example

```python
from plexsphere.models.label_selector_ast_node import LabelSelectorASTNode

# TODO update the JSON string below
json = "{}"
# create an instance of LabelSelectorASTNode from a JSON string
label_selector_ast_node_instance = LabelSelectorASTNode.from_json(json)
# print the JSON string representation of the object
print(LabelSelectorASTNode.to_json())

# convert the object into a dict
label_selector_ast_node_dict = label_selector_ast_node_instance.to_dict()
# create an instance of LabelSelectorASTNode from a dict
label_selector_ast_node_from_dict = LabelSelectorASTNode.from_dict(label_selector_ast_node_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


