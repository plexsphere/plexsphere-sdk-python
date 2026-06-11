# RegisterRequest

Body for POST /v1/register. Carries every field a freshly-installed plexd agent submits at registration time: the tenant scope (`project_id` + `resource_id`), the one-shot BootstrapToken plaintext, a replay-protection nonce, and the agent's WireGuard X25519 public key.  Field order in the wire shape mirrors the `internal/identity/nodes/registration.RegisterCommand` value-object the handler builds before invoking the application service. The handler validates `public_key` BEFORE attempting any BootstrapToken consumption so a malformed key cannot also waste a token. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**project_id** | **UUID** | Project the redeeming substrate is enrolling into. Must match the &#x60;project_id&#x60; segment of the plaintext &#x60;bootstrap_token&#x60;; a mismatch surfaces as 403 with &#x60;reason&#x3D;project_mismatch&#x60;.  | 
**resource_id** | **str** | Human-readable Resource handle (e.g. &#x60;edge-router-01&#x60;) the registering Node binds to. The handler resolves this against the Resource aggregate; an unknown handle surfaces as 404 with &#x60;code&#x3D;resource_not_found&#x60;.  | 
**bootstrap_token** | **str** | Plaintext BootstrapToken in the documented &#x60;psb_&lt;env&gt;_&lt;projectId&gt;_&lt;kind&gt;_&lt;random&gt;&#x60; format (matches &#x60;^psb_[a-z]+_[a-z2-7]+_(node|bridge)_[a-z2-7]{20,}$&#x60;) . The Validator parses the prefix to discover the env + project-id + kind triple before running the candidate scan.  | 
**nonce** | **str** | Request-side replay-protection nonce. The persistence layer holds a partial UNIQUE on (project_id, consumed_nonce); a duplicate surfaces as 403 with &#x60;reason&#x3D;nonce_collision&#x60;.  | 
**public_key** | **str** | 32-byte X25519 (WireGuard) public key, base64-encoded with standard padding. RFC 7748 §5 fixes the curve to a 32-byte little-endian point encoding so any other byte length surfaces as 400 with &#x60;code&#x3D;public_key_invalid&#x60;; the all-zero degenerate value (a known small-order point) is also rejected with the same code. The fixed length-44 base64 canonical form is the standard &#x60;RawStdEncoding&#x60;-with-padding result for a 32-byte payload.  | 
**requested_resource_id** | **str** | Optional override the operator can supply when the substrate&#39;s own naming differs from the platform handle (e.g. the Node identifies itself as &#x60;node-12&#x60; while the platform Resource is &#x60;edge-router-01&#x60;). Empty or omitted means \&quot;no override; use &#x60;resource_id&#x60; verbatim\&quot; .  | [optional] 

## Example

```python
from plexsphere.models.register_request import RegisterRequest

# TODO update the JSON string below
json = "{}"
# create an instance of RegisterRequest from a JSON string
register_request_instance = RegisterRequest.from_json(json)
# print the JSON string representation of the object
print(RegisterRequest.to_json())

# convert the object into a dict
register_request_dict = register_request_instance.to_dict()
# create an instance of RegisterRequest from a dict
register_request_from_dict = RegisterRequest.from_dict(register_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


