# CapabilityRow

One per-Node capability inventory row returned by `ListCapabilities` — a projection of the Node's reported capability manifest paired with its checksum status. It carries the binary metadata, the checksum verdict, and the built-in action inventory the agent advertised; the hook lists are not projected here (see the Hook Catalog surfaces). 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**node_id** | **UUID** | Node the capability row describes (UUIDv7). | 
**domain_id** | **UUID** | Owning Domain (UUIDv7) — the residency pivot the per-row visibility filter authorises against.  | 
**binary_version** | **str** | Reported plexd agent version string of the Node&#39;s running binary.  | 
**binary_checksum** | **bytes** | SHA-256 digest of the running binary, 32 bytes base64-encoded with standard padding. Absent when the Node has not yet reported a checksum.  | [optional] 
**status** | [**CapabilityStatus**](CapabilityStatus.md) |  | 
**builtin_actions** | [**List[BuiltinAction]**](BuiltinAction.md) | The built-in actions the Node&#39;s agent advertised on its most recent capability manifest, in the order it reported them. This is the read side of &#x60;CapabilityManifestRequest.builtin_actions&#x60; — what an operator surface reads to offer a Node&#39;s actual actions rather than a fixed list.  Required, so every row carries the key and a client needs no null-check: a Node that reported no inventory renders the empty array. That is deliberately NOT the same claim as \&quot;this Node supports no actions\&quot; — an agent that predates the field and one that reports an empty list are indistinguishable here. Do not treat an empty array as a reason to refuse a dispatch; the Node remains the authority on what it can run.  | 
**reported_at** | **datetime** | Timestamp at which the capability manifest snapshot was last reported (UTC).  | 

## Example

```python
from plexsphere.models.capability_row import CapabilityRow

# TODO update the JSON string below
json = "{}"
# create an instance of CapabilityRow from a JSON string
capability_row_instance = CapabilityRow.from_json(json)
# print the JSON string representation of the object
print(CapabilityRow.to_json())

# convert the object into a dict
capability_row_dict = capability_row_instance.to_dict()
# create an instance of CapabilityRow from a dict
capability_row_from_dict = CapabilityRow.from_dict(capability_row_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


