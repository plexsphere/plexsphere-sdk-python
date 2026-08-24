# TelemetryRouteList

The Telemetry Routes one Project holds, returned by `GET /v1/projects/{id}/telemetry-routes` in creation order. A Project holds a bounded number of routes, so the set is not paginated. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**items** | [**List[TelemetryRoute]**](TelemetryRoute.md) | The Telemetry Routes the Project holds. | 

## Example

```python
from plexsphere.models.telemetry_route_list import TelemetryRouteList

# TODO update the JSON string below
json = "{}"
# create an instance of TelemetryRouteList from a JSON string
telemetry_route_list_instance = TelemetryRouteList.from_json(json)
# print the JSON string representation of the object
print(TelemetryRouteList.to_json())

# convert the object into a dict
telemetry_route_list_dict = telemetry_route_list_instance.to_dict()
# create an instance of TelemetryRouteList from a dict
telemetry_route_list_from_dict = TelemetryRouteList.from_dict(telemetry_route_list_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


