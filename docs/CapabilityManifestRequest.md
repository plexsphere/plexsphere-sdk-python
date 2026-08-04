# CapabilityManifestRequest

Body for PUT /v1/nodes/{id}/capabilities. Carries the per-Node capability manifest snapshot plexd reports on agent boot and whenever the running binary, host-key, declared hooks, or built-in action inventory change: the agent binary semver, the SHA-256 of the running binary (for tamper-evidence and rollout tracking), the optional SSH host-key fingerprint (for the downstream integrity correlator), the optional list of hooks the agent advertises, and the optional inventory of built-in actions the agent implements. The handler canonicalises the envelope through the `tenancy.NewCapabilityManifest` value-object constructor; every invariant is enforced at the aggregate boundary before the recorder ever touches Postgres. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**binary_version** | **str** | Human-readable plexd agent version string (e.g. &#x60;plexd-v0.4.2-ge5f3a1c&#x60;). Non-empty after trimming whitespace — the handler rejects an empty value with 400 &#x60;binary_version_empty&#x60;.  | 
**binary_checksum** | **bytes** | SHA-256 digest of the running plexd binary, 32 bytes base64-encoded with standard padding. Anything that does not decode to exactly 32 bytes is rejected with 400 &#x60;binary_checksum_invalid&#x60;.  | 
**ssh_host_key_fingerprint** | **str** | Optional OpenSSH SHA-256 host-key fingerprint of the running agent, rendered as &#x60;SHA256:&lt;base64&gt;&#x60;. Empty or absent values are accepted; a non-empty value that does not match the canonical form is rejected with 400 &#x60;ssh_host_key_fingerprint_invalid&#x60;. Downstream integrity correlators branch on &#x60;host_key_changed&#x60; in the response to detect a host-key rotation between snapshots.  | [optional] 
**declared_hooks** | [**List[DeclaredHook]**](DeclaredHook.md) | Optional list of hook declarations the agent advertises. Each entry pairs a hook name with the SHA-256 digest of the hook payload so the integrity correlator can detect a hook-content change without re-fetching the payload. The manifest invariants reject duplicate names (400 &#x60;declared_hook_duplicate&#x60;), more than 128 entries (400 &#x60;declared_hooks_too_many&#x60;), and any per-entry violation (400 &#x60;declared_hook_invalid&#x60;).  | [optional] 
**plexd_hooks** | [**List[PlexdHook]**](PlexdHook.md) | Optional list of Kubernetes PlexdHook custom resources the agent discovered in its cluster and advertises read-only. Distinct from &#x60;declared_hooks&#x60;: each entry carries an OCI image digest, a free-form parameter map, an execution timeout, and a sandbox flag rather than a payload checksum. Discovery is read-only — plexsphere records what plexd observed and never writes PlexdHook objects back. The manifest invariants reject duplicate names (422 &#x60;plexd_hook_duplicate&#x60;), more than 128 entries (422 &#x60;plexd_hooks_too_many&#x60;), and any per-entry violation (422 &#x60;plexd_hook_invalid&#x60;).  | [optional] 
**builtin_actions** | [**List[BuiltinAction]**](BuiltinAction.md) | Optional inventory of the built-in actions the agent implements — the operations &#x60;POST /v1/projects/{project_id}/resources/{resource_id}/actions&#x60; can dispatch against this Node with &#x60;kind: builtin&#x60;. Each entry names an action, describes it, and declares the parameters it accepts.  The inventory is advisory and read-only. Dispatch does not consult it: an execution names its action, the Node is the authority on whether it can run it, and an unknown action fails at the Node. What the inventory buys is the ability to answer \&quot;what can this Node do?\&quot; without dispatching anything — so an operator surface can offer a Node&#39;s actual actions rather than a fixed list, and so a fleet-wide capability query has data to read.  Absent and empty are distinct in intent but not in effect: an agent that reports no inventory is simply one the platform cannot answer that question for. It is not an agent whose actions are known to be none, and nothing refuses a dispatch on that basis.  The manifest invariants reject duplicate action names (422 &#x60;builtin_action_duplicate&#x60;), more than 128 entries (422 &#x60;builtin_actions_too_many&#x60;), and any per-entry violation (422 &#x60;builtin_action_invalid&#x60;).  | [optional] 

## Example

```python
from plexsphere.models.capability_manifest_request import CapabilityManifestRequest

# TODO update the JSON string below
json = "{}"
# create an instance of CapabilityManifestRequest from a JSON string
capability_manifest_request_instance = CapabilityManifestRequest.from_json(json)
# print the JSON string representation of the object
print(CapabilityManifestRequest.to_json())

# convert the object into a dict
capability_manifest_request_dict = capability_manifest_request_instance.to_dict()
# create an instance of CapabilityManifestRequest from a dict
capability_manifest_request_from_dict = CapabilityManifestRequest.from_dict(capability_manifest_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


