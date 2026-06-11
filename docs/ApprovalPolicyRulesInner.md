# ApprovalPolicyRulesInner


## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**action_kind** | **str** | Action kind the rule matches. Must be non-empty for the rule to be valid.  | 
**target_resource** | **str** | Optional resource the rule narrows to. An empty value is a wildcard that matches any target for the given action kind.  | [optional] 
**approvers_required** | **int** | Number of distinct approvers the matched action requires. Must be at least one.  | 

## Example

```python
from plexsphere.models.approval_policy_rules_inner import ApprovalPolicyRulesInner

# TODO update the JSON string below
json = "{}"
# create an instance of ApprovalPolicyRulesInner from a JSON string
approval_policy_rules_inner_instance = ApprovalPolicyRulesInner.from_json(json)
# print the JSON string representation of the object
print(ApprovalPolicyRulesInner.to_json())

# convert the object into a dict
approval_policy_rules_inner_dict = approval_policy_rules_inner_instance.to_dict()
# create an instance of ApprovalPolicyRulesInner from a dict
approval_policy_rules_inner_from_dict = ApprovalPolicyRulesInner.from_dict(approval_policy_rules_inner_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


