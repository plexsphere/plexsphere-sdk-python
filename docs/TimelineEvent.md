# TimelineEvent

One entry on an incident's append-only timeline. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**id** | **UUID** | Stable identifier of the timeline event (UUIDv7). | 
**incident_id** | **UUID** | Incident the event belongs to. | 
**kind** | [**TimelineEventKind**](TimelineEventKind.md) |  | 
**message** | **str** | Free-form message recorded on the event. | 
**occurred_at** | **datetime** | RFC 3339 instant the event occurred. The timeline is ordered by this field ascending.  | 

## Example

```python
from plexsphere.models.timeline_event import TimelineEvent

# TODO update the JSON string below
json = "{}"
# create an instance of TimelineEvent from a JSON string
timeline_event_instance = TimelineEvent.from_json(json)
# print the JSON string representation of the object
print(TimelineEvent.to_json())

# convert the object into a dict
timeline_event_dict = timeline_event_instance.to_dict()
# create an instance of TimelineEvent from a dict
timeline_event_from_dict = TimelineEvent.from_dict(timeline_event_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


