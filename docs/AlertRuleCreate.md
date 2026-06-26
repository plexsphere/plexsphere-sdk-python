# AlertRuleCreate

Body for storing a new alert rule. The rule is stored, managed configuration only and is not evaluated in this phase. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**name** | **str** | Domain-unique logical name. A duplicate name in the Domain is rejected with 409.  | 
**signal** | **str** | The metric or log series expression the rule records as its watched signal.  | 
**comparator** | [**AlertComparator**](AlertComparator.md) |  | 
**threshold** | **float** | Finite value the signal is recorded against. NaN and infinity are rejected.  | 
**severity** | [**AlertSeverity**](AlertSeverity.md) |  | 
**enabled** | **bool** | Whether the rule is recorded as enabled. Defaults to enabled. The flag is stored configuration only and does not cause evaluation.  | [optional] [default to True]

## Example

```python
from plexsphere.models.alert_rule_create import AlertRuleCreate

# TODO update the JSON string below
json = "{}"
# create an instance of AlertRuleCreate from a JSON string
alert_rule_create_instance = AlertRuleCreate.from_json(json)
# print the JSON string representation of the object
print(AlertRuleCreate.to_json())

# convert the object into a dict
alert_rule_create_dict = alert_rule_create_instance.to_dict()
# create an instance of AlertRuleCreate from a dict
alert_rule_create_from_dict = AlertRuleCreate.from_dict(alert_rule_create_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


