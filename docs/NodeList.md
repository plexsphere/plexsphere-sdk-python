# NodeList

Page of Nodes returned by `GET /v1/nodes`. Per-row visibility is layered on top of the persistence-level page so `items` is the subset the caller is authorised to see. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**items** | [**List[NodeSummary]**](NodeSummary.md) | Nodes in the current page. | 
**next_cursor** | **str** | Continuation token for the next page. &#x60;null&#x60; or omitted when the iteration has reached end-of-stream. The encoding is HMAC-signed by the server so a tampered cursor surfaces as &#x60;400&#x60; on the next call.  | [optional] 

## Example

```python
from plexsphere.models.node_list import NodeList

# TODO update the JSON string below
json = "{}"
# create an instance of NodeList from a JSON string
node_list_instance = NodeList.from_json(json)
# print the JSON string representation of the object
print(NodeList.to_json())

# convert the object into a dict
node_list_dict = node_list_instance.to_dict()
# create an instance of NodeList from a dict
node_list_from_dict = NodeList.from_dict(node_list_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


