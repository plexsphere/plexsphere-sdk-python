# BridgeConfigUpdatedPayload

Wire payload carried by a `bridge_config_updated` SSE envelope. The controller emits one envelope per (Node, bridge Resource) delivered-config projection whenever the bridge orchestrator persists a new effective configuration, and one empty-config envelope per previously-addressed Node when a bridge Resource is deleted. The payload mirrors the `NodeStateBridge` block plexd consumes on the reconciliation-pull path so the same reconcile loop applies the wire delta and the snapshot delta without branching. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**node_id** | **UUID** | Addressed Node the delivered-config projection targets. Equals the &#x60;{id}&#x60; path parameter of the SSE subscription the envelope is delivered on.  | 
**bridge_resource_id** | **UUID** | Identifier of the bridge Resource aggregate the delivered-config projection was derived from. Two bridge Resources addressing the same Node produce two envelopes carrying distinct &#x60;bridge_resource_id&#x60; values; the consumer merges them by sorting on &#x60;bridge_resource_id&#x60; ascending.  | 
**effective_config** | [**NodeStateBridge**](NodeStateBridge.md) | The per-Node bridge-orchestrator delivered-config block the addressed Node MUST program for the named bridge Resource. Each child block is present-but-nullable so plexd&#39;s reconcile loop can diff by field presence; an all-null block is a legitimate state — it means the Node no longer matches the bridge Resource (e.g. the Resource was deleted or its selector no longer captures the Node).  | 

## Example

```python
from plexsphere.models.bridge_config_updated_payload import BridgeConfigUpdatedPayload

# TODO update the JSON string below
json = "{}"
# create an instance of BridgeConfigUpdatedPayload from a JSON string
bridge_config_updated_payload_instance = BridgeConfigUpdatedPayload.from_json(json)
# print the JSON string representation of the object
print(BridgeConfigUpdatedPayload.to_json())

# convert the object into a dict
bridge_config_updated_payload_dict = bridge_config_updated_payload_instance.to_dict()
# create an instance of BridgeConfigUpdatedPayload from a dict
bridge_config_updated_payload_from_dict = BridgeConfigUpdatedPayload.from_dict(bridge_config_updated_payload_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


