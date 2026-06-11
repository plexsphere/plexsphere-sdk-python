# NodeSummary

Hydrated summary projection of a Node aggregate. The shape is shared by `ListNodes`; richer per-Node read surfaces (events, state, heartbeat, reachability) live under `/v1/nodes/{id}/...`.  `kind` and `name` are optional today: they are projected from the `Resource` aggregate the `Node` row references, and the per-row read in `GET /v1/nodes` does not yet join the `resources` table. Until that join lands the fields are omitted from the wire envelope; clients MUST treat them as absent rather than as empty strings. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**id** | **UUID** | Node identifier (UUIDv7). | 
**name** | **str** | Human-readable Node name, sourced from the &#x60;Resource&#x60; aggregate the Node row references. Optional — see the schema-level note above for the deferred projection contract.  | [optional] 
**domain_id** | **UUID** | Parent Domain identifier (UUIDv7). | 
**kind** | **str** | Node kind discriminator, sourced from the &#x60;Resource&#x60; aggregate the Node row references. Today the cluster recognises &#x60;vm&#x60;, &#x60;bridge&#x60;, and &#x60;worker&#x60;; the field is intentionally modelled as an open string rather than an enum so a future kind addition does not require a contract version bump. Optional — see the schema-level note above for the deferred projection contract.  | [optional] 
**created_at** | **datetime** | Aggregate creation timestamp (UTC). | 

## Example

```python
from plexsphere.models.node_summary import NodeSummary

# TODO update the JSON string below
json = "{}"
# create an instance of NodeSummary from a JSON string
node_summary_instance = NodeSummary.from_json(json)
# print the JSON string representation of the object
print(NodeSummary.to_json())

# convert the object into a dict
node_summary_dict = node_summary_instance.to_dict()
# create an instance of NodeSummary from a dict
node_summary_from_dict = NodeSummary.from_dict(node_summary_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


