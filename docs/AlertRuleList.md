# AlertRuleList

Cursor-paginated page of stored alert rules. `next_cursor` is the value to pass back as the `cursor` query parameter on the next call; it is absent when the page is short — the end-of-stream signal callers stop on. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**items** | [**List[AlertRule]**](AlertRule.md) | The stored alert rules on this page. | 
**next_cursor** | **str** | Opaque pagination cursor for the next page, or absent when the page is the last.  | [optional] 

## Example

```python
from plexsphere.models.alert_rule_list import AlertRuleList

# TODO update the JSON string below
json = "{}"
# create an instance of AlertRuleList from a JSON string
alert_rule_list_instance = AlertRuleList.from_json(json)
# print the JSON string representation of the object
print(AlertRuleList.to_json())

# convert the object into a dict
alert_rule_list_dict = alert_rule_list_instance.to_dict()
# create an instance of AlertRuleList from a dict
alert_rule_list_from_dict = AlertRuleList.from_dict(alert_rule_list_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


