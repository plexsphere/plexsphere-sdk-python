# Incident

A per-Domain operational incident with its append-only timeline. A resolved incident carries a `resolved_at` timestamp; an open incident does not. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**id** | **UUID** | Stable identifier of the incident (UUIDv7). | 
**domain_id** | **UUID** | Domain the incident is scoped to. | 
**title** | **str** | Short human-readable title of the incident. | 
**severity** | [**IncidentSeverity**](IncidentSeverity.md) |  | 
**status** | [**IncidentStatus**](IncidentStatus.md) |  | 
**opened_at** | **datetime** | RFC 3339 instant the incident was opened. | 
**resolved_at** | **datetime** | RFC 3339 instant the incident was resolved, or absent while it is still open.  | [optional] 
**timeline** | [**List[TimelineEvent]**](TimelineEvent.md) | The incident&#39;s append-only timeline, ordered by &#x60;occurred_at&#x60; ascending. The list projection omits the timeline; the single-incident read includes it.  | 

## Example

```python
from plexsphere.models.incident import Incident

# TODO update the JSON string below
json = "{}"
# create an instance of Incident from a JSON string
incident_instance = Incident.from_json(json)
# print the JSON string representation of the object
print(Incident.to_json())

# convert the object into a dict
incident_dict = incident_instance.to_dict()
# create an instance of Incident from a dict
incident_from_dict = Incident.from_dict(incident_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


