# TelemetryRoute

Where one Project's telemetry on one signal goes: the predicate that selects records and the sinks that receive them. A signal the Project routes nowhere falls back to the platform's own backends. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**id** | **UUID** | Stable identifier of the route (UUIDv7). | 
**project_id** | **UUID** | Project the route belongs to. Immutable once the route exists.  | 
**signal** | [**TelemetrySignal**](TelemetrySignal.md) |  | 
**severity_floor** | [**TelemetrySeverity**](TelemetrySeverity.md) | Least severe log keyword the route carries. Absent means the route states no floor and carries every record on its signal. Present on a &#x60;logs&#x60; route only.  | [optional] 
**name_prefix** | **str** | Leading characters of the metric names the route selects. Absent means the route states no prefix. Present on a &#x60;metrics&#x60; route only.  | [optional] 
**sink_ids** | **List[UUID]** | Sinks the route delivers to. At least one and at most sixteen, no zero id, and no sink named twice.  | 
**created_at** | **datetime** | RFC 3339 timestamp the route was created. | 
**updated_at** | **datetime** | RFC 3339 timestamp the route was last changed. | 

## Example

```python
from plexsphere.models.telemetry_route import TelemetryRoute

# TODO update the JSON string below
json = "{}"
# create an instance of TelemetryRoute from a JSON string
telemetry_route_instance = TelemetryRoute.from_json(json)
# print the JSON string representation of the object
print(TelemetryRoute.to_json())

# convert the object into a dict
telemetry_route_dict = telemetry_route_instance.to_dict()
# create an instance of TelemetryRoute from a dict
telemetry_route_from_dict = TelemetryRoute.from_dict(telemetry_route_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


