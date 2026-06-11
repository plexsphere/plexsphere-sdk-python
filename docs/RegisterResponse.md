# RegisterResponse

Response body for POST /v1/register. Bundles every artifact plexd needs to finish its bootstrap: the freshly-allocated `node_id` + `mesh_ip`, the Domain's signing public key + key id (for SSE event verification), the per-Node NSK plaintext (returned EXACTLY ONCE — see the `x-plexsphere-once: true` marker on the `nsk` field), the initial wireguard peer snapshot for table programming, and the Domain's mesh CIDR for routing-table programming.  Operators MUST capture the `nsk` plaintext from this response and persist it locally on the registering Node — no API surface ever returns it again. The Spectral `plexsphere-write-once-post-must-be-issue-response` rule polices the `x-plexsphere-once: true` marker so the plaintext cannot accidentally appear on any other operation . 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**node_id** | **UUID** | Freshly-allocated Node aggregate id (UUIDv7). Echoed in the response so plexd records its own identity.  | 
**mesh_ip** | **str** | Allocator-assigned mesh address inside the Domain&#39;s mesh CIDR. Canonical &#x60;netip.Addr&#x60; string form (&#x60;100.64.0.1&#x60;, never zero-padded).  | 
**signing_public_key** | **str** | Domain&#39;s active signing public key, base64-encoded. Plexd pins this and uses it to verify signed SSE events delivered.  | 
**signing_key_id** | **str** | Identifier (kid) the Domain&#39;s active signing key is indexed under (e.g. a DID-URL fragment). Carried so plexd can rotate-aware verify SSE events whose header references a non-current kid during a rotation grace window.  | 
**nsk** | **str** | Per-Node Node Secret Key plaintext (32 bytes, base64- encoded). **Shown exactly once** — there is no API surface that ever returns the plaintext again because the persistence layer stores only the wrapped form. Operators MUST capture it from this response and pin it on the registering Node out-of-band. The Spectral rule polices this guarantee against any future schema that would leak the marker outside this single response.  | 
**peer_snapshot** | [**List[RegisterPeer]**](RegisterPeer.md) | Initial wireguard peer set the registering Node needs to bring its table up. Empty for the first Node enrolled into a Domain. Subsequent membership churn is delivered through the SSE stream rather than re-issued here.  | 
**domain_mesh_cidr** | **str** | Domain&#39;s mesh CIDR (e.g. &#x60;100.64.0.0/10&#x60;). Carried so plexd can program its routing table without a follow-up Domain lookup.  | 

## Example

```python
from plexsphere.models.register_response import RegisterResponse

# TODO update the JSON string below
json = "{}"
# create an instance of RegisterResponse from a JSON string
register_response_instance = RegisterResponse.from_json(json)
# print the JSON string representation of the object
print(RegisterResponse.to_json())

# convert the object into a dict
register_response_dict = register_response_instance.to_dict()
# create an instance of RegisterResponse from a dict
register_response_from_dict = RegisterResponse.from_dict(register_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


