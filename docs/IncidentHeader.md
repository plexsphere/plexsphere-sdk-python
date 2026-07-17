# IncidentHeader

The timeline-free header projection of an incident — the shape the list surface returns. A resolved incident carries a `resolved_at` timestamp; an open incident does not. 

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

## Example

```python
from plexsphere.models.incident_header import IncidentHeader

# TODO update the JSON string below
json = "{}"
# create an instance of IncidentHeader from a JSON string
incident_header_instance = IncidentHeader.from_json(json)
# print the JSON string representation of the object
print(IncidentHeader.to_json())

# convert the object into a dict
incident_header_dict = incident_header_instance.to_dict()
# create an instance of IncidentHeader from a dict
incident_header_from_dict = IncidentHeader.from_dict(incident_header_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


