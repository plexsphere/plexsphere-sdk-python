# HeartbeatRequest

Body for POST /v1/nodes/{id}/heartbeat. Carries the per-Node liveness signal plexd emits at a 30s cadence: the client wall-clock (for skew detection), the SHA-256 of the running plexd binary (for tamper-evidence and rollout tracking), the human-readable plexd version string, and a free-form NAT summary that the controller correlates with the per-Domain bridge view in later stories. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**client_now** | **datetime** | Client wall-clock timestamp at the moment the heartbeat was assembled. The handler rejects the request with 400 &#x60;clock_skew&#x60; if the value drifts more than 60 seconds from server now.  | 
**binary_checksum** | **bytes** | SHA-256 digest of the running plexd binary. The wire form is the 32-byte raw digest encoded as either lowercase hex (64 characters) or base64 with standard padding (44 characters); the handler decodes both forms and rejects anything else with 400 &#x60;binary_checksum_empty&#x60;.  | 
**binary_version** | **str** | Human-readable plexd version string (e.g. &#x60;plexd-v0.4.2-ge5f3a1c&#x60;). Non-empty per the application- boundary invariants — the handler rejects an empty value with 400 &#x60;binary_version_empty&#x60;.  | 
**nat_summary** | **Dict[str, object]** | Free-form NAT discovery summary produced by plexd. Empty object is permitted — later stories define the schema once the per-Domain bridge orchestrator lands .  | 

## Example

```python
from plexsphere.models.heartbeat_request import HeartbeatRequest

# TODO update the JSON string below
json = "{}"
# create an instance of HeartbeatRequest from a JSON string
heartbeat_request_instance = HeartbeatRequest.from_json(json)
# print the JSON string representation of the object
print(HeartbeatRequest.to_json())

# convert the object into a dict
heartbeat_request_dict = heartbeat_request_instance.to_dict()
# create an instance of HeartbeatRequest from a dict
heartbeat_request_from_dict = HeartbeatRequest.from_dict(heartbeat_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


