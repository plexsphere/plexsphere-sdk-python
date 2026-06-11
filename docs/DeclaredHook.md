# DeclaredHook

Single hook declaration inside a CapabilityManifestRequest's `declared_hooks` list. The pair (name, checksum) lets the integrity correlator detect a hook-content change without re-fetching the hook payload. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**name** | **str** | Hook identifier (e.g. &#x60;post-install&#x60;). Non-empty after trimming whitespace; a violation surfaces as 400 &#x60;declared_hook_invalid&#x60;.  | 
**checksum** | **bytes** | SHA-256 digest of the hook payload, 32 bytes base64-encoded with standard padding. Anything that does not decode to exactly 32 bytes surfaces as 400 &#x60;declared_hook_invalid&#x60;.  | 

## Example

```python
from plexsphere.models.declared_hook import DeclaredHook

# TODO update the JSON string below
json = "{}"
# create an instance of DeclaredHook from a JSON string
declared_hook_instance = DeclaredHook.from_json(json)
# print the JSON string representation of the object
print(DeclaredHook.to_json())

# convert the object into a dict
declared_hook_dict = declared_hook_instance.to_dict()
# create an instance of DeclaredHook from a dict
declared_hook_from_dict = DeclaredHook.from_dict(declared_hook_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


