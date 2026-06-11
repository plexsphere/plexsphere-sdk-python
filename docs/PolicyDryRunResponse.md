# PolicyDryRunResponse

Response for the Policy dry-run endpoint. Reports the Project Nodes the candidate selector matches, the structural diff against the Policy's current head revision, and the count of peer pairs whose connectivity verdict would change. `unreachable_node_ids` lists matched Nodes the calling subject cannot read — opaque identifiers only, never display names — so the editor can warn the operator about cross-references it cannot resolve. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**matched_node_ids** | **List[UUID]** |  | 
**rule_diff** | [**PolicyDiff**](PolicyDiff.md) |  | 
**peer_pairs_affected** | **int** |  | 
**unreachable_node_ids** | **List[UUID]** |  | 

## Example

```python
from plexsphere.models.policy_dry_run_response import PolicyDryRunResponse

# TODO update the JSON string below
json = "{}"
# create an instance of PolicyDryRunResponse from a JSON string
policy_dry_run_response_instance = PolicyDryRunResponse.from_json(json)
# print the JSON string representation of the object
print(PolicyDryRunResponse.to_json())

# convert the object into a dict
policy_dry_run_response_dict = policy_dry_run_response_instance.to_dict()
# create an instance of PolicyDryRunResponse from a dict
policy_dry_run_response_from_dict = PolicyDryRunResponse.from_dict(policy_dry_run_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


