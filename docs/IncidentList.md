# IncidentList

Cursor-paginated page of incident headers. The headers omit the per-incident timeline. `next_cursor` is the value to pass back as the `cursor` query parameter on the next call; it is absent when the page is short. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**items** | [**List[Incident]**](Incident.md) | The incident headers on this page. | 
**next_cursor** | **str** | Opaque pagination cursor for the next page, or absent when the page is the last.  | [optional] 

## Example

```python
from plexsphere.models.incident_list import IncidentList

# TODO update the JSON string below
json = "{}"
# create an instance of IncidentList from a JSON string
incident_list_instance = IncidentList.from_json(json)
# print the JSON string representation of the object
print(IncidentList.to_json())

# convert the object into a dict
incident_list_dict = incident_list_instance.to_dict()
# create an instance of IncidentList from a dict
incident_list_from_dict = IncidentList.from_dict(incident_list_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


