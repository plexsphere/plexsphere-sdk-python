# ManagedPushTarget

Display-only read projection of a Domain's managed-push target. NEVER carries the kubeconfig ciphertext or plaintext — only the hex SHA-256 fingerprint of the plaintext for display and drift checks. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**enabled** | **bool** | Whether managed pushes are admitted against this target. | 
**api_server_url** | **str** | API-server URL of the managed-push cluster, parsed from the attached kubeconfig.  | 
**kubeconfig_sha256** | **str** | Hex-encoded SHA-256 fingerprint of the kubeconfig plaintext. Display-only — lets an operator confirm which kubeconfig is sealed without exposing its contents.  | 
**created_at** | **datetime** | Timestamp the target was first attached. | 
**updated_at** | **datetime** | Timestamp the target was last replaced. | 

## Example

```python
from plexsphere.models.managed_push_target import ManagedPushTarget

# TODO update the JSON string below
json = "{}"
# create an instance of ManagedPushTarget from a JSON string
managed_push_target_instance = ManagedPushTarget.from_json(json)
# print the JSON string representation of the object
print(ManagedPushTarget.to_json())

# convert the object into a dict
managed_push_target_dict = managed_push_target_instance.to_dict()
# create an instance of ManagedPushTarget from a dict
managed_push_target_from_dict = ManagedPushTarget.from_dict(managed_push_target_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


