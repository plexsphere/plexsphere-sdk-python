# DiscoveredHook

One discovered Kubernetes hook custom resource returned by `ListHooks` — a read-only discovery projection per Node: what the agent observed in its cluster, with a flag for whether the discovered image digest matches the declared one. plexsphere never writes hook objects back. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**node_id** | **UUID** | Node the hook was discovered on (UUIDv7). | 
**domain_id** | **UUID** | Owning Domain (UUIDv7) — the residency pivot the per-row visibility filter authorises against.  | 
**name** | **str** | Discovered hook resource name. | 
**image_digest** | **str** | OCI image digest of the discovered hook image in canonical &#x60;sha256:&lt;64 lowercase hex&gt;&#x60; form.  | 
**image_digest_match** | **bool** | &#x60;true&#x60; when the discovered image digest matches the digest declared in the Node&#39;s capability manifest, &#x60;false&#x60; when it diverges.  | 

## Example

```python
from plexsphere.models.discovered_hook import DiscoveredHook

# TODO update the JSON string below
json = "{}"
# create an instance of DiscoveredHook from a JSON string
discovered_hook_instance = DiscoveredHook.from_json(json)
# print the JSON string representation of the object
print(DiscoveredHook.to_json())

# convert the object into a dict
discovered_hook_dict = discovered_hook_instance.to_dict()
# create an instance of DiscoveredHook from a dict
discovered_hook_from_dict = DiscoveredHook.from_dict(discovered_hook_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


