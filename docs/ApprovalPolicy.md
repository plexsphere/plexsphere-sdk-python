# ApprovalPolicy

Dual-control policy stored on the owning Domain's `approval_policy` JSONB column. It declares which proposed actions require approval. An empty policy (no `rules`, marshalled as `{}`) is the wired-but-empty default that gates nothing — the machinery is in place but no action is held for approval until an operator adds a rule. This schema documents the stored column shape; it is not the body of any approval operation. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**rules** | [**List[ApprovalPolicyRulesInner]**](ApprovalPolicyRulesInner.md) | Matching rules. A nil or empty list is the empty policy and gates no action.  | [optional] 

## Example

```python
from plexsphere.models.approval_policy import ApprovalPolicy

# TODO update the JSON string below
json = "{}"
# create an instance of ApprovalPolicy from a JSON string
approval_policy_instance = ApprovalPolicy.from_json(json)
# print the JSON string representation of the object
print(ApprovalPolicy.to_json())

# convert the object into a dict
approval_policy_dict = approval_policy_instance.to_dict()
# create an instance of ApprovalPolicy from a dict
approval_policy_from_dict = ApprovalPolicy.from_dict(approval_policy_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


