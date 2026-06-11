# CapabilityManifestResponse

Body for PUT /v1/nodes/{id}/capabilities success responses. Carries the server-side commit timestamp and the diff against the prior persisted snapshot: `fields_changed` enumerates the manifest fields that transitioned (sorted alphabetically so the wire response is byte-stable across runs), and `host_key_changed` is broken out as its own boolean because SSH host-key transitions are security-load-bearing and downstream consumers branch on the flag without parsing `fields_changed`. An idempotent PUT (no diff) emits an empty `fields_changed` array and `host_key_changed: false`. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**accepted_at** | **datetime** | Server-side timestamp at which the capability manifest snapshot was committed.  | 
**fields_changed** | **List[str]** | Alphabetically-sorted list of manifest fields that transitioned versus the prior persisted snapshot. Possible entries: &#x60;binary_version&#x60;, &#x60;binary_checksum&#x60;, &#x60;ssh_host_key_fingerprint&#x60;, &#x60;declared_hooks&#x60;, &#x60;plexd_hooks&#x60;. Empty on an idempotent no-op PUT.  | 
**host_key_changed** | **bool** | &#x60;true&#x60; when the SSH host-key fingerprint transitioned versus the prior persisted snapshot (including a transition into or out of the unset state). Always present so downstream security correlators can branch on the flag without parsing &#x60;fields_changed&#x60;.  | 

## Example

```python
from plexsphere.models.capability_manifest_response import CapabilityManifestResponse

# TODO update the JSON string below
json = "{}"
# create an instance of CapabilityManifestResponse from a JSON string
capability_manifest_response_instance = CapabilityManifestResponse.from_json(json)
# print the JSON string representation of the object
print(CapabilityManifestResponse.to_json())

# convert the object into a dict
capability_manifest_response_dict = capability_manifest_response_instance.to_dict()
# create an instance of CapabilityManifestResponse from a dict
capability_manifest_response_from_dict = CapabilityManifestResponse.from_dict(capability_manifest_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


