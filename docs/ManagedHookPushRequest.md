# ManagedHookPushRequest

Body for `POST /v1/domains/{domainId}/managed-push/hooks`. Applies one PlexdHook object to the Domain's managed-push cluster. The PlexdHook spec fields mirror the discovery-only PlexdHook projection. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**namespace** | **str** | Target namespace the PlexdHook is applied into. | 
**name** | **str** | PlexdHook object name. | 
**image_digest** | **str** | Pinned image digest the hook executes — a &#x60;sha256:&#x60; content address, never a mutable tag.  | 
**parameters** | **Dict[str, str]** | Opaque string-to-string parameter map forwarded to the hook.  | [optional] 
**timeout_seconds** | **int** | Per-invocation timeout in seconds. Zero or omitted leaves the agent&#39;s default in force.  | [optional] 
**sandbox** | **bool** | Whether the hook runs in the agent&#39;s sandboxed execution mode.  | [optional] 

## Example

```python
from plexsphere.models.managed_hook_push_request import ManagedHookPushRequest

# TODO update the JSON string below
json = "{}"
# create an instance of ManagedHookPushRequest from a JSON string
managed_hook_push_request_instance = ManagedHookPushRequest.from_json(json)
# print the JSON string representation of the object
print(ManagedHookPushRequest.to_json())

# convert the object into a dict
managed_hook_push_request_dict = managed_hook_push_request_instance.to_dict()
# create an instance of ManagedHookPushRequest from a dict
managed_hook_push_request_from_dict = ManagedHookPushRequest.from_dict(managed_hook_push_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


