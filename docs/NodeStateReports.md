# NodeStateReports

Per-Node node-state block carried on the `NodeStateSnapshot` envelope. The addressed Node's node-state entries are fanned into three buckets by kind: operator-visible, platform-owned `metadata`; platform-owned `data`; and upstream, workload-authored `reports`. Each bucket is a required array of `StateEntry` and is `[]` (never `null`) when the Node carries no entry of that kind, so plexd's reconcile loop can diff by field presence. Entries within a bucket are ordered by `key` ascending so two consecutive pulls against the same ledger snapshot are byte-equal. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**metadata** | [**List[StateEntry]**](StateEntry.md) | Operator-visible, platform-owned metadata entries for the addressed Node, ordered by &#x60;key&#x60; ascending.  | 
**data** | [**List[StateEntry]**](StateEntry.md) | Platform-owned operational data entries for the addressed Node, ordered by &#x60;key&#x60; ascending.  | 
**reports** | [**List[StateEntry]**](StateEntry.md) | Upstream, workload-authored report entries for the addressed Node, ordered by &#x60;key&#x60; ascending.  | 

## Example

```python
from plexsphere.models.node_state_reports import NodeStateReports

# TODO update the JSON string below
json = "{}"
# create an instance of NodeStateReports from a JSON string
node_state_reports_instance = NodeStateReports.from_json(json)
# print the JSON string representation of the object
print(NodeStateReports.to_json())

# convert the object into a dict
node_state_reports_dict = node_state_reports_instance.to_dict()
# create an instance of NodeStateReports from a dict
node_state_reports_from_dict = NodeStateReports.from_dict(node_state_reports_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


