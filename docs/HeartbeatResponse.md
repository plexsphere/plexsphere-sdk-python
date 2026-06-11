# HeartbeatResponse

Body for POST /v1/nodes/{id}/heartbeat success responses . Carries the server-side commit timestamp and two reconciliation hints. both flags default to `false`; later stories flip them when the controller wants the caller to issue a fresh reconciliation pull or rotate its NSK. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**accepted_at** | **datetime** | Server-side timestamp at which the heartbeat fact was committed. Distinct from &#x60;client_now&#x60; so the caller can estimate one-way latency and clock-skew.  | 
**reconcile** | **bool** | When &#x60;true&#x60;, the controller is asking the caller to issue a fresh &#x60;GET /v1/nodes/{id}/state&#x60; reconciliation pull because the snapshot the caller is operating against has drifted. Defaults to &#x60;false&#x60; — later stories flip this flag when the controller observes a divergence .  | 
**rotate_keys** | **bool** | When &#x60;true&#x60;, a mesh-key rotation is pending for the heartbeating Node: the caller MUST generate a fresh Curve25519 keypair and complete the rotation via &#x60;POST /v1/keys/rotate&#x60;. This flag is load-bearing — it is the heartbeat-channel half of the two-channel rotation dispatch (the SSE &#x60;rotate_keys&#x60; event is the other half), so a Node that reconnected rather than holding a live SSE stream still learns it must rotate within one heartbeat interval. &#x60;false&#x60; when no &#x60;peer_key_rotation&#x60; row is pending for the Node.  | 

## Example

```python
from plexsphere.models.heartbeat_response import HeartbeatResponse

# TODO update the JSON string below
json = "{}"
# create an instance of HeartbeatResponse from a JSON string
heartbeat_response_instance = HeartbeatResponse.from_json(json)
# print the JSON string representation of the object
print(HeartbeatResponse.to_json())

# convert the object into a dict
heartbeat_response_dict = heartbeat_response_instance.to_dict()
# create an instance of HeartbeatResponse from a dict
heartbeat_response_from_dict = HeartbeatResponse.from_dict(heartbeat_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


