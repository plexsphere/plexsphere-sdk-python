# AlertRule

A stored alert rule for a Domain. Alert rules are stored, managed configuration only: the platform records and serves them and does not evaluate them in this phase. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**id** | **UUID** | Stable identifier of the alert rule (UUIDv7). | 
**domain_id** | **UUID** | Domain the alert rule is scoped to. | 
**name** | **str** | Domain-unique logical name the rule is addressed by.  | 
**signal** | **str** | The metric or log series expression the rule records as its watched signal.  | 
**comparator** | [**AlertComparator**](AlertComparator.md) |  | 
**threshold** | **float** | Finite value the signal is recorded against. NaN and infinity are rejected at creation.  | 
**severity** | [**AlertSeverity**](AlertSeverity.md) |  | 
**enabled** | **bool** | Whether the rule is recorded as enabled. The flag is stored configuration only and does not cause the platform to evaluate the rule in this phase.  | 
**created_at** | **datetime** | RFC 3339 timestamp the rule was first stored. | 
**updated_at** | **datetime** | RFC 3339 timestamp the rule was last updated. | 

## Example

```python
from plexsphere.models.alert_rule import AlertRule

# TODO update the JSON string below
json = "{}"
# create an instance of AlertRule from a JSON string
alert_rule_instance = AlertRule.from_json(json)
# print the JSON string representation of the object
print(AlertRule.to_json())

# convert the object into a dict
alert_rule_dict = alert_rule_instance.to_dict()
# create an instance of AlertRule from a dict
alert_rule_from_dict = AlertRule.from_dict(alert_rule_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


