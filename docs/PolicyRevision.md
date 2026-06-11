# PolicyRevision

Append-only entry in a Policy's version history. Each PATCH on the parent Policy lands a new Revision row; the head pointer on the Policy aggregate advances atomically inside the same transaction. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**id** | **UUID** | Revision identifier (UUIDv7). | 
**policy_id** | **UUID** | Parent Policy identifier. | 
**parent_id** | **UUID** | Predecessor revision identifier. &#x60;null&#x60; on the initial revision of a fresh Policy; non-null on every subsequent revision.  | [optional] 
**selector** | [**PolicySelector**](PolicySelector.md) |  | 
**rules** | [**List[PolicyRule]**](PolicyRule.md) | Ordered rule list at this revision. Capped at 1024 entries by the aggregate; the cap is mirrored as a CHECK constraint in the persistence layer so a wire-level oversize is rejected before it reaches the editor.  | 
**created_at** | **datetime** |  | 
**created_by** | **UUID** | Identifier of the principal that authored the revision.  | 
**correlation_id** | **UUID** | Upstream request correlation identifier propagated into the audit row and outbox event. &#x60;null&#x60; when the revision was authored outside an inbound HTTP request.  | [optional] 

## Example

```python
from plexsphere.models.policy_revision import PolicyRevision

# TODO update the JSON string below
json = "{}"
# create an instance of PolicyRevision from a JSON string
policy_revision_instance = PolicyRevision.from_json(json)
# print the JSON string representation of the object
print(PolicyRevision.to_json())

# convert the object into a dict
policy_revision_dict = policy_revision_instance.to_dict()
# create an instance of PolicyRevision from a dict
policy_revision_from_dict = PolicyRevision.from_dict(policy_revision_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


