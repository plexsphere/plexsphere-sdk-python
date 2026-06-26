# IncidentOpenRequest

Body for opening a new incident. The incident is opened in the `open` state with an empty timeline. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**title** | **str** | Short human-readable title of the incident. | 
**severity** | [**IncidentSeverity**](IncidentSeverity.md) |  | 

## Example

```python
from plexsphere.models.incident_open_request import IncidentOpenRequest

# TODO update the JSON string below
json = "{}"
# create an instance of IncidentOpenRequest from a JSON string
incident_open_request_instance = IncidentOpenRequest.from_json(json)
# print the JSON string representation of the object
print(IncidentOpenRequest.to_json())

# convert the object into a dict
incident_open_request_dict = incident_open_request_instance.to_dict()
# create an instance of IncidentOpenRequest from a dict
incident_open_request_from_dict = IncidentOpenRequest.from_dict(incident_open_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


