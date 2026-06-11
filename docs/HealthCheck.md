# HealthCheck

Single dependency probe result.

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**name** | **str** | Stable identifier of the checked dependency. | 
**status** | **str** | Outcome of the probe (ok, degraded, down). | 
**detail** | **str** | Human-readable context for the outcome. | 

## Example

```python
from plexsphere.models.health_check import HealthCheck

# TODO update the JSON string below
json = "{}"
# create an instance of HealthCheck from a JSON string
health_check_instance = HealthCheck.from_json(json)
# print the JSON string representation of the object
print(HealthCheck.to_json())

# convert the object into a dict
health_check_dict = health_check_instance.to_dict()
# create an instance of HealthCheck from a dict
health_check_from_dict = HealthCheck.from_dict(health_check_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


