# Policy

Network Policy aggregate. Each Policy belongs to exactly one Project (the ReBAC parent), carries an append-only revision history, and has at any moment exactly one live head revision. The head pointer advances atomically inside the same transaction that lands a new revision; a concurrent PATCH that loses the partial-unique-head race surfaces as `409 revision_conflict`. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**id** | **UUID** | Policy identifier (UUIDv7). | 
**project_id** | **UUID** | Owning Project (UUIDv7). | 
**slug** | **str** | Stable kebab-case handle for the Policy. Unique within the owning Project — a duplicate &#x60;(project_id, slug)&#x60; rejects with &#x60;409 policy_slug_taken&#x60;.  | 
**display_name** | **str** | Human-readable label. Optional — falls back to &#x60;slug&#x60; in renderings when empty.  | [optional] 
**head_revision_id** | **UUID** | Identifier of the current head revision. Advances every PATCH inside the same transaction that inserts the new revision row.  | 
**created_at** | **datetime** |  | 
**updated_at** | **datetime** | Timestamp of the last head-revision advance. Equal to &#x60;created_at&#x60; on a freshly-created Policy.  | 
**head** | [**PolicyRevision**](PolicyRevision.md) | Fully-hydrated head revision. Returned on &#x60;GET&#x60; of a single Policy; intentionally omitted from list entries to keep the page payload bounded — callers that need rule lists per row iterate the list and then &#x60;GET&#x60; each Policy.  | [optional] 

## Example

```python
from plexsphere.models.policy import Policy

# TODO update the JSON string below
json = "{}"
# create an instance of Policy from a JSON string
policy_instance = Policy.from_json(json)
# print the JSON string representation of the object
print(Policy.to_json())

# convert the object into a dict
policy_dict = policy_instance.to_dict()
# create an instance of Policy from a dict
policy_from_dict = Policy.from_dict(policy_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


