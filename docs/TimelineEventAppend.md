# TimelineEventAppend

Body for appending an event to an incident's timeline. Events may only be appended while the incident is open. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**kind** | [**TimelineEventKind**](TimelineEventKind.md) |  | 
**message** | **str** | Free-form message to record on the event. | 

## Example

```python
from plexsphere.models.timeline_event_append import TimelineEventAppend

# TODO update the JSON string below
json = "{}"
# create an instance of TimelineEventAppend from a JSON string
timeline_event_append_instance = TimelineEventAppend.from_json(json)
# print the JSON string representation of the object
print(TimelineEventAppend.to_json())

# convert the object into a dict
timeline_event_append_dict = timeline_event_append_instance.to_dict()
# create an instance of TimelineEventAppend from a dict
timeline_event_append_from_dict = TimelineEventAppend.from_dict(timeline_event_append_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


