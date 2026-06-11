# IntegrityViolationRequest

Single integrity-violation report inside an `IntegrityViolationsRequest`. The `kind` field discriminates between the three artifact classes the agent monitors. For `binary_checksum` and `hook_checksum` the entry carries the observed and (optionally) expected 32-byte SHA-256 digests; for `ssh_host_key` it carries the observed and (optionally) expected `SHA256:<base64>` fingerprints. Cross-kind column leakage (a `binary_checksum` carrying a fingerprint, or an `ssh_host_key` carrying a checksum) is rejected with 400 `integrity_violation_kind_mismatch`. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**kind** | **str** | Artifact class the divergence applies to. A value outside this closed set is rejected with 400 &#x60;integrity_violation_kind_invalid&#x60;.  | 
**detected_by** | **str** | On-Node detector that surfaced the violation: the agent&#39;s startup self-check, an inotify-triggered re-check during normal operation, or a pre-dispatch verification before running a hook. A value outside this closed set is rejected with 400 &#x60;integrity_violation_detected_by_invalid&#x60;.  | 
**artifact_id** | **str** | Stable identifier of the affected artifact (hook name, binary path label, or host-key file label). Non-empty after trimming whitespace; a violation surfaces as 400 &#x60;integrity_violation_artifact_id_empty&#x60;.  | 
**observed_checksum** | **bytes** | SHA-256 digest the agent observed for the artifact, 32 bytes base64-encoded with standard padding. REQUIRED for the &#x60;binary_checksum&#x60; and &#x60;hook_checksum&#x60; kinds; MUST be empty or absent for the &#x60;ssh_host_key&#x60; kind. Anything that does not decode to exactly 32 bytes (when expected) is rejected with 400 &#x60;integrity_violation_checksum_invalid&#x60;. A non-empty value on an &#x60;ssh_host_key&#x60; entry surfaces as 400 &#x60;integrity_violation_kind_mismatch&#x60;.  | [optional] 
**expected_checksum** | **bytes** | Optional SHA-256 digest the agent expected for the artifact, 32 bytes base64-encoded. Accepted only for the &#x60;binary_checksum&#x60; and &#x60;hook_checksum&#x60; kinds. When present, anything that does not decode to exactly 32 bytes is rejected with 400 &#x60;integrity_violation_checksum_invalid&#x60;.  | [optional] 
**observed_fingerprint** | **str** | OpenSSH SHA-256 host-key fingerprint the agent observed, rendered as &#x60;SHA256:&lt;base64&gt;&#x60;. REQUIRED for the &#x60;ssh_host_key&#x60; kind; MUST be empty or absent for the checksum kinds. A non-empty value on a checksum kind surfaces as 400 &#x60;integrity_violation_kind_mismatch&#x60;; a value that does not match the canonical form surfaces as 400 &#x60;integrity_violation_host_key_fingerprint_invalid&#x60;.  | [optional] 
**expected_fingerprint** | **str** | Optional OpenSSH SHA-256 host-key fingerprint the agent expected, rendered as &#x60;SHA256:&lt;base64&gt;&#x60;. Accepted only for the &#x60;ssh_host_key&#x60; kind. A value that does not match the canonical form is rejected with 400 &#x60;integrity_violation_host_key_fingerprint_invalid&#x60;.  | [optional] 

## Example

```python
from plexsphere.models.integrity_violation_request import IntegrityViolationRequest

# TODO update the JSON string below
json = "{}"
# create an instance of IntegrityViolationRequest from a JSON string
integrity_violation_request_instance = IntegrityViolationRequest.from_json(json)
# print the JSON string representation of the object
print(IntegrityViolationRequest.to_json())

# convert the object into a dict
integrity_violation_request_dict = integrity_violation_request_instance.to_dict()
# create an instance of IntegrityViolationRequest from a dict
integrity_violation_request_from_dict = IntegrityViolationRequest.from_dict(integrity_violation_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


