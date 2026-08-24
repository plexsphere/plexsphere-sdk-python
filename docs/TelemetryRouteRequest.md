# TelemetryRouteRequest

Body for `POST /v1/projects/{id}/telemetry-routes` and `PUT /v1/telemetry-routes/{id}`. On the update it is the full post-image: a field absent from the body is cleared, not left untouched.  `project_id` is absent from the body because the stored value is immutable — the create takes the Project from the path, and the update applies against the Project the route already belongs to. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**signal** | [**TelemetrySignal**](TelemetrySignal.md) |  | 
**severity_floor** | [**TelemetrySeverity**](TelemetrySeverity.md) | Least severe log keyword to carry. Stated on a &#x60;logs&#x60; route only; stating it on another signal is refused with &#x60;422 telemetry_route_invalid&#x60; rather than carried along and ignored at delivery.  | [optional] 
**name_prefix** | **str** | Leading characters of the metric names to select. Stated on a &#x60;metrics&#x60; route only; stating it on another signal is refused with &#x60;422 telemetry_route_invalid&#x60;.  | [optional] 
**sink_ids** | **List[UUID]** | Sinks to deliver to. At least one and at most sixteen, no zero id, and no sink named twice. The ceiling bounds the delivery fan-out a single batch pays for, which the per-Project route limit alone does not. Each target is checked at authoring time, so a route that would deliver nowhere is refused where it is written.  | 

## Example

```python
from plexsphere.models.telemetry_route_request import TelemetryRouteRequest

# TODO update the JSON string below
json = "{}"
# create an instance of TelemetryRouteRequest from a JSON string
telemetry_route_request_instance = TelemetryRouteRequest.from_json(json)
# print the JSON string representation of the object
print(TelemetryRouteRequest.to_json())

# convert the object into a dict
telemetry_route_request_dict = telemetry_route_request_instance.to_dict()
# create an instance of TelemetryRouteRequest from a dict
telemetry_route_request_from_dict = TelemetryRouteRequest.from_dict(telemetry_route_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


