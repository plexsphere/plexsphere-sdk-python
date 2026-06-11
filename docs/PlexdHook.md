# PlexdHook

Single discovered Kubernetes PlexdHook custom resource inside a CapabilityManifestRequest's `plexd_hooks` list. Distinct from `DeclaredHook` (the declared script hook): a PlexdHook carries an OCI image digest, a free-form parameter map, an execution timeout, and a sandbox flag rather than a payload checksum. Discovery is read-only on the plexsphere side — the manifest only records what plexd observed in the customer cluster; plexsphere never writes PlexdHook objects back nor resolves the digest against a registry. Per-entry invariants surface as 422 `plexd_hook_invalid`. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**name** | **str** | PlexdHook resource name (e.g. &#x60;nightly-backup&#x60;). Non-empty after trimming whitespace; a violation surfaces as 422 &#x60;plexd_hook_invalid&#x60;.  | 
**image_digest** | **str** | OCI image digest of the hook image in canonical &#x60;sha256:&lt;64 lowercase hex&gt;&#x60; form. Format-only validation — plexsphere performs no registry lookup. Anything that does not match surfaces as 422 &#x60;plexd_hook_invalid&#x60;.  | 
**parameters** | **Dict[str, str]** | Optional free-form string-to-string parameter map copied verbatim from the discovered PlexdHook spec.  | [optional] 
**timeout_seconds** | **int** | Optional execution timeout in whole seconds. Non-negative; a negative value surfaces as 422 &#x60;plexd_hook_invalid&#x60;. The domain owns this as a duration — the &#x60;_seconds&#x60; suffix is the wire encoding only.  | [optional] 
**sandbox** | **bool** | Optional flag indicating the discovered hook requests sandboxed execution. Absent is treated as &#x60;false&#x60;.  | [optional] 

## Example

```python
from plexsphere.models.plexd_hook import PlexdHook

# TODO update the JSON string below
json = "{}"
# create an instance of PlexdHook from a JSON string
plexd_hook_instance = PlexdHook.from_json(json)
# print the JSON string representation of the object
print(PlexdHook.to_json())

# convert the object into a dict
plexd_hook_dict = plexd_hook_instance.to_dict()
# create an instance of PlexdHook from a dict
plexd_hook_from_dict = PlexdHook.from_dict(plexd_hook_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


