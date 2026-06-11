# Execution

Metadata-only projection of an action Execution. The shape is shared by `DispatchExecution`, `ListExecutions`, and `GetExecution`. It carries the action identity, the owning Project and Domain, the aggregate status, and the per-Node targets — but no secret or token columns. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**id** | **UUID** | Execution identifier (UUIDv7). | 
**project_id** | **UUID** | Identifier of the owning Project (UUIDv7). | 
**domain_id** | **UUID** | Identifier of the owning Domain (UUIDv7) — the residency pivot the per-Domain concurrent-execution budget is counted against.  | 
**action** | **str** | Name of the dispatched action. | 
**type** | [**ActionKind**](ActionKind.md) |  | 
**status** | [**ExecutionStatus**](ExecutionStatus.md) | Aggregate status of the Execution, derived from its per-Node targets.  | 
**created_at** | **datetime** | Aggregate creation timestamp (UTC). | 
**updated_at** | **datetime** | Timestamp of the Execution&#39;s most recent advance (UTC). | 
**targets** | [**List[ExecutionTarget]**](ExecutionTarget.md) | Per-Node invocations the Execution fanned out to. | 

## Example

```python
from plexsphere.models.execution import Execution

# TODO update the JSON string below
json = "{}"
# create an instance of Execution from a JSON string
execution_instance = Execution.from_json(json)
# print the JSON string representation of the object
print(Execution.to_json())

# convert the object into a dict
execution_dict = execution_instance.to_dict()
# create an instance of Execution from a dict
execution_from_dict = Execution.from_dict(execution_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


