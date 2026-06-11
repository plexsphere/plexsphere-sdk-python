# NodeStatePolicy

Per-Node compiled policy fan-out block. Carries the merged rule list the controller projected for the addressed Node, the base64-encoded SHA-256 fingerprint that plexd uses to short-circuit reconcile when the locally-applied ruleset already matches, and the revision identifier of the policy-id-ascending first matched Policy. The block is `null` on the `NodeStateSnapshot` envelope when no compiled ruleset row exists for the addressed Node (the per-Node selector did not match any active Policy revision).  Multi-policy semantics: the controller persists one `plexsphere.policy_compiled_ruleset` row per (Node, Policy) tuple. When a Node matches multiple Policies, the snapshot composer merges those rows by sorting on `policy_id` and concatenating each row's `rule_payload` bytes in ascending order. `rules` decodes the merged byte stream into the canonical wire shape; `fingerprint` is `sha256.Sum256(payload_0 || payload_1 || ...)` over the same byte stream; `revision_id` is drawn from the policy-id-ascending first row so a single uuid remains on the wire. Plexd treats the fingerprint as an opaque comparison key — it is stable for a given (Node, set of matched Policy revisions, rule_payload byte streams) tuple and changes whenever any contributing row changes. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**revision_id** | **UUID** | Identifier of the Policy revision the merged ruleset anchors on — drawn from the policy-id-ascending first row when the addressed Node matches multiple Policies. A Node matching a single Policy carries that Policy&#39;s revision id directly.  | 
**fingerprint** | **bytes** | SHA-256 digest over the merged &#x60;rule_payload&#x60; byte stream the controller persisted in &#x60;plexsphere.policy_compiled_ruleset&#x60;. Exactly 32 bytes once decoded; on the wire the field is base64-encoded with standard padding (44 ASCII characters). For a Node matching a single Policy the digest equals &#x60;sha256.Sum256(rule_payload)&#x60;; for a Node matching multiple Policies it equals &#x60;sha256.Sum256(payload_0 || payload_1 || ...)&#x60; with &#x60;payload_i&#x60; ordered by ascending &#x60;policy_id&#x60;. The digest is an opaque comparison key — plexd compares it byte-for-byte against its locally-applied ruleset&#39;s stored digest and does not re-derive it from the &#x60;rules&#x60; array.  | 
**rules** | [**List[NodeStateCompiledRule]**](NodeStateCompiledRule.md) | Canonical, deterministically-sorted L3/L4 rule list the addressed Node MUST program. For multi-policy matches the array is the per-Policy concatenation of each row&#39;s canonical rule list in policy-id-ascending order. Empty array is a legitimate state — it means the Node matches at least one Policy&#39;s selector pair but no Policy contributed any rules (or every rule was filtered out).  | 

## Example

```python
from plexsphere.models.node_state_policy import NodeStatePolicy

# TODO update the JSON string below
json = "{}"
# create an instance of NodeStatePolicy from a JSON string
node_state_policy_instance = NodeStatePolicy.from_json(json)
# print the JSON string representation of the object
print(NodeStatePolicy.to_json())

# convert the object into a dict
node_state_policy_dict = node_state_policy_instance.to_dict()
# create an instance of NodeStatePolicy from a dict
node_state_policy_from_dict = NodeStatePolicy.from_dict(node_state_policy_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


