# CapabilityRow

One per-Node capability inventory row returned by `ListCapabilities` — a metadata-only projection of the Node's reported capability manifest paired with its checksum status. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**node_id** | **UUID** | Node the capability row describes (UUIDv7). | 
**domain_id** | **UUID** | Owning Domain (UUIDv7) — the residency pivot the per-row visibility filter authorises against.  | 
**binary_version** | **str** | Reported plexd agent version string of the Node&#39;s running binary.  | 
**binary_checksum** | **bytes** | SHA-256 digest of the running binary, 32 bytes base64-encoded with standard padding. Absent when the Node has not yet reported a checksum.  | [optional] 
**status** | [**CapabilityStatus**](CapabilityStatus.md) |  | 
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


