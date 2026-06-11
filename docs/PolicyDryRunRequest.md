# PolicyDryRunRequest

Body for the Policy dry-run endpoint. Carries a candidate selector + rule list to evaluate against the current Project state. Evaluation is read-only — no rows are written, no events are emitted, and no audit entry is appended for the evaluation itself. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**selector** | [**PolicySelector**](PolicySelector.md) |  | 
**rules** | [**List[PolicyRule]**](PolicyRule.md) |  | 

## Example

```python
from plexsphere.models.policy_dry_run_request import PolicyDryRunRequest

# TODO update the JSON string below
json = "{}"
# create an instance of PolicyDryRunRequest from a JSON string
policy_dry_run_request_instance = PolicyDryRunRequest.from_json(json)
# print the JSON string representation of the object
print(PolicyDryRunRequest.to_json())

# convert the object into a dict
policy_dry_run_request_dict = policy_dry_run_request_instance.to_dict()
# create an instance of PolicyDryRunRequest from a dict
policy_dry_run_request_from_dict = PolicyDryRunRequest.from_dict(policy_dry_run_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


