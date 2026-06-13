# ManagedHookPush

Read projection of one recorded managed-hook push. NEVER echoes the prior object content — exposes only a `had_prior_state` boolean so a subsequent rollback knows whether to restore or delete. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**id** | **UUID** | Recorded push identifier (UUIDv7). | 
**domain_id** | **UUID** | Owning Domain identifier. | 
**hook_name** | **str** | Name of the PlexdHook object applied. | 
**namespace** | **str** | Namespace the hook was applied into. | 
**status** | **str** | Lifecycle status of the push. &#x60;applied&#x60; is the initial state after the object lands on the cluster; &#x60;rolled_back&#x60; is the terminal state after a successful rollback.  | 
**had_prior_state** | **bool** | Whether the apply replaced an existing object. When &#x60;true&#x60; a rollback restores the prior object; when &#x60;false&#x60; a rollback deletes the applied object.  | 
**pushed_at** | **datetime** | Timestamp the object was applied. | 
**rolled_back_at** | **datetime** | Timestamp the push was rolled back, or &#x60;null&#x60; when the push has not been rolled back.  | [optional] 

## Example

```python
from plexsphere.models.managed_hook_push import ManagedHookPush

# TODO update the JSON string below
json = "{}"
# create an instance of ManagedHookPush from a JSON string
managed_hook_push_instance = ManagedHookPush.from_json(json)
# print the JSON string representation of the object
print(ManagedHookPush.to_json())

# convert the object into a dict
managed_hook_push_dict = managed_hook_push_instance.to_dict()
# create an instance of ManagedHookPush from a dict
managed_hook_push_from_dict = ManagedHookPush.from_dict(managed_hook_push_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


