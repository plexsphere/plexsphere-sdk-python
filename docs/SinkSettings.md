# SinkSettings

Per-type configuration a destination needs beyond its address. Every field is stated on one sink type only; stating one on another type is refused with `422 sink_invalid`. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**dataset** | **str** | Logical stream an &#x60;otlp&#x60; destination files the delivered telemetry under. Stated on &#x60;otlp&#x60; sinks only.  | [optional] 

## Example

```python
from plexsphere.models.sink_settings import SinkSettings

# TODO update the JSON string below
json = "{}"
# create an instance of SinkSettings from a JSON string
sink_settings_instance = SinkSettings.from_json(json)
# print the JSON string representation of the object
print(SinkSettings.to_json())

# convert the object into a dict
sink_settings_dict = sink_settings_instance.to_dict()
# create an instance of SinkSettings from a dict
sink_settings_from_dict = SinkSettings.from_dict(sink_settings_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


