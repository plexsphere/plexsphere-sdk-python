# PolicyUpdatedPayload

Wire payload carried by a `policy_updated` SSE envelope. The controller emits one envelope per per-(Node, Policy) compiled-ruleset row whenever the compile arm persists a new revision, and one empty-rules envelope per previously- addressed Node when a Policy is deleted. The payload fields mirror the `NodeStatePolicy` block plexd consumes on the reconciliation-pull path so the same rule programmer applies the wire delta and the snapshot delta without branching.  The `fingerprint` field is the base64-encoded SHA-256 digest over the canonical `rule_payload` byte stream the controller persisted; plexd compares it byte-for-byte against the locally-applied ruleset's digest and short-circuits reconcile when the two match. Empty `rules` is a legitimate state — a policy-deletion fan-out emits `rules: []` and the canonical empty-ruleset fingerprint so a consumer observing only the wire stream converges to the same projection as a consumer that re-reads the snapshot column. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**node_id** | **UUID** | Addressed Node the compiled-ruleset projection targets. Equals the &#x60;{id}&#x60; path parameter of the SSE subscription the envelope is delivered on.  | 
**policy_id** | **UUID** | Identifier of the Policy aggregate the compiled-ruleset row was projected from. Two Policies addressing the same Node produce two envelopes carrying distinct &#x60;policy_id&#x60; values; the consumer merges them by sorting on &#x60;policy_id&#x60; ascending.  | 
**revision_id** | **UUID** | Identifier of the Policy revision the compiled ruleset anchors on. Equals the head revision id at compile time; re-emission for the same revision is idempotent under the publisher&#39;s deduplication window.  | 
**fingerprint** | **bytes** | SHA-256 digest over the canonical &#x60;rule_payload&#x60; byte stream the controller persisted. Exactly 32 bytes once decoded; on the wire the field is base64-encoded with standard padding (44 ASCII characters). The digest is an opaque comparison key — plexd compares it byte-for-byte against its locally-applied ruleset&#39;s stored digest and does not re-derive it from the &#x60;rules&#x60; array.  | 
**rules** | [**List[NodeStateCompiledRule]**](NodeStateCompiledRule.md) | Canonical, deterministically-sorted L3/L4 rule list the addressed Node MUST program for the named Policy. An empty array is a legitimate state — it means the Node no longer matches the Policy (e.g. the Policy was deleted or its selector no longer captures the Node).  | 

## Example

```python
from plexsphere.models.policy_updated_payload import PolicyUpdatedPayload

# TODO update the JSON string below
json = "{}"
# create an instance of PolicyUpdatedPayload from a JSON string
policy_updated_payload_instance = PolicyUpdatedPayload.from_json(json)
# print the JSON string representation of the object
print(PolicyUpdatedPayload.to_json())

# convert the object into a dict
policy_updated_payload_dict = policy_updated_payload_instance.to_dict()
# create an instance of PolicyUpdatedPayload from a dict
policy_updated_payload_from_dict = PolicyUpdatedPayload.from_dict(policy_updated_payload_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


