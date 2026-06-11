# DispatchExecutionRequest

Body for `POST /v1/projects/{project_id}/executions:dispatch`. It names the action to dispatch, the parameters to pass, and the target set. The target is EXACTLY ONE of a single `node_id` or an opaque label `selector` — supplying both, or neither, is rejected. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**action** | **str** | Name of the action to dispatch. | 
**type** | [**ActionKind**](ActionKind.md) | Kind of action to dispatch (&#x60;builtin&#x60; or &#x60;hook&#x60;). Defaults to &#x60;builtin&#x60; when omitted.  | [optional] 
**parameters** | **Dict[str, object]** | Opaque JSON parameter document passed verbatim to the action.  | [optional] 
**timeout_seconds** | **int** | Per-dispatch execution timeout in whole seconds. A background reconciler times out an invocation whose Node never reports a terminal result within this window.  | [optional] 
**selector** | **str** | Opaque label selector resolving the target cohort. Supply EXACTLY ONE of &#x60;selector&#x60; or &#x60;node_id&#x60;; supplying both, or neither, is rejected.  | [optional] 
**node_id** | **UUID** | Identifier of a single target Node (UUIDv7). Supply EXACTLY ONE of &#x60;node_id&#x60; or &#x60;selector&#x60;; supplying both, or neither, is rejected.  | [optional] 

## Example

```python
from plexsphere.models.dispatch_execution_request import DispatchExecutionRequest

# TODO update the JSON string below
json = "{}"
# create an instance of DispatchExecutionRequest from a JSON string
dispatch_execution_request_instance = DispatchExecutionRequest.from_json(json)
# print the JSON string representation of the object
print(DispatchExecutionRequest.to_json())

# convert the object into a dict
dispatch_execution_request_dict = dispatch_execution_request_instance.to_dict()
# create an instance of DispatchExecutionRequest from a dict
dispatch_execution_request_from_dict = DispatchExecutionRequest.from_dict(dispatch_execution_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


