# NodeStateSnapshot

Canonical reconciliation-pull envelope for a single Node . The four wire blocks — `peers`, `policy`, `bridge`, `state`, and `reports` — are always present so plexd's reconcile loop can diff by field presence rather than absence; later stories (policy fan-out, bridge orchestrator, node-state reports) populate the currently-empty blocks without changing the wire shape. Empty `peers` is `[]` (never `null`); the other blocks may be `null` until their owning story lands. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**peers** | [**List[NodeStatePeer]**](NodeStatePeer.md) | Peer set the addressed Node should program into its WireGuard table. One entry per other Node in the addressed Node&#39;s Domain — the addressed Node itself is excluded so plexd does not program a self-peer . Ordered by &#x60;node_id&#x60; ascending so two consecutive pulls against the same ledger snapshot are byte-equal.  | 
**reachability** | [**Reachability**](Reachability.md) | Latest &#x60;Reachability&#x60; projection for the addressed Node, carried inside the reconciliation-pull payload so plexd sees the same health view that &#x60;GET /v1/nodes/{id}/reachability&#x60; exposes without an additional round-trip.  | 
**policy** | [**NodeStatePolicy**](NodeStatePolicy.md) | Policy block — present-but-empty placeholder populates the wire shape. May be &#x60;null&#x60; until then; the field itself is always present so plexd&#39;s reconcile loop can diff by field presence.  | 
**bridge** | [**NodeStateBridge**](NodeStateBridge.md) | Bridge orchestrator block — present-but-empty placeholder  populates the wire shape. May be &#x60;null&#x60; until then; the field itself is always present.  | 
**state** | [**NodeStateReports**](NodeStateReports.md) | Node-state block for the addressed Node: its node-state entries fanned into the &#x60;metadata&#x60;, &#x60;data&#x60;, and &#x60;reports&#x60; buckets of &#x60;NodeStateReports&#x60;. Populated after a successful reconciliation pull — each bucket is &#x60;[]&#x60; (never &#x60;null&#x60;) when the Node carries no entry of that kind. The field is always present so plexd&#39;s reconcile loop can diff by field presence; it is &#x60;null&#x60; only when the snapshot carries no node-state projection at all.  | 
**reports** | [**NodeStateReports**](NodeStateReports.md) | Forward-compatibility mirror of &#x60;state&#x60;: the controller points it at the same &#x60;NodeStateReports&#x60; value so the convergence with the SSE event taxonomy stays explicit and a future split between live state and rolled-up reports lands without an OpenAPI break. Always equals &#x60;state&#x60; today. The field is always present and is &#x60;null&#x60; only when &#x60;state&#x60; is.  | 

## Example

```python
from plexsphere.models.node_state_snapshot import NodeStateSnapshot

# TODO update the JSON string below
json = "{}"
# create an instance of NodeStateSnapshot from a JSON string
node_state_snapshot_instance = NodeStateSnapshot.from_json(json)
# print the JSON string representation of the object
print(NodeStateSnapshot.to_json())

# convert the object into a dict
node_state_snapshot_dict = node_state_snapshot_instance.to_dict()
# create an instance of NodeStateSnapshot from a dict
node_state_snapshot_from_dict = NodeStateSnapshot.from_dict(node_state_snapshot_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


