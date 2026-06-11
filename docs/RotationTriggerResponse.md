# RotationTriggerResponse

Body for POST /v1/nodes/{id}/keys/rotate success responses. Carries the rotation handle an operator uses to track a triggered mesh-key rotation: the `peer_key_rotation` row id, the live Peer the rotation targets, and whether the response reflects a pre-existing in-flight rotation. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**rotation_id** | **UUID** | Identifier of the &#x60;peer_key_rotation&#x60; row the request recorded — freshly inserted, or the in-flight row when &#x60;already_pending&#x60; is true.  | 
**peer_id** | **UUID** | Identifier of the live Peer whose mesh key the rotation targets.  | 
**already_pending** | **bool** | True when the response reflects a pre-existing pending rotation — an idempotent re-request that surfaced the in-flight row — rather than a freshly recorded request. Operators switch on this flag to distinguish a newly dispatched &#x60;rotate_keys&#x60; command from an idempotent no-op.  | 

## Example

```python
from plexsphere.models.rotation_trigger_response import RotationTriggerResponse

# TODO update the JSON string below
json = "{}"
# create an instance of RotationTriggerResponse from a JSON string
rotation_trigger_response_instance = RotationTriggerResponse.from_json(json)
# print the JSON string representation of the object
print(RotationTriggerResponse.to_json())

# convert the object into a dict
rotation_trigger_response_dict = rotation_trigger_response_instance.to_dict()
# create an instance of RotationTriggerResponse from a dict
rotation_trigger_response_from_dict = RotationTriggerResponse.from_dict(rotation_trigger_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


