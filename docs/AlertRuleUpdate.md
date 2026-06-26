# AlertRuleUpdate

Partial update for a stored alert rule. Every field is optional; an absent field leaves the stored value untouched. The rule remains stored, managed configuration only and is not evaluated in this phase. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**name** | **str** | New Domain-unique logical name. A rename onto an existing name in the Domain is rejected with 409.  | [optional] 
**signal** | **str** | New watched signal expression. | [optional] 
**comparator** | [**AlertComparator**](AlertComparator.md) |  | [optional] 
**threshold** | **float** | New finite threshold value. | [optional] 
**severity** | [**AlertSeverity**](AlertSeverity.md) |  | [optional] 
**enabled** | **bool** | New enabled flag. Stored configuration only; does not cause evaluation.  | [optional] 

## Example

```python
from plexsphere.models.alert_rule_update import AlertRuleUpdate

# TODO update the JSON string below
json = "{}"
# create an instance of AlertRuleUpdate from a JSON string
alert_rule_update_instance = AlertRuleUpdate.from_json(json)
# print the JSON string representation of the object
print(AlertRuleUpdate.to_json())

# convert the object into a dict
alert_rule_update_dict = alert_rule_update_instance.to_dict()
# create an instance of AlertRuleUpdate from a dict
alert_rule_update_from_dict = AlertRuleUpdate.from_dict(alert_rule_update_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


