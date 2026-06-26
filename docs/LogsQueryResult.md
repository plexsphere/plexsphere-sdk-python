# LogsQueryResult

Result of a logs query, projected from the logs backend's response. `status_code` is the backend's HTTP status and `body` is the backend's verbatim JSON payload so the Dashboard can render the streams without the platform re-encoding them. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**status_code** | **int** | HTTP status the logs backend returned. | 
**body** | **str** | The logs backend&#39;s verbatim response body, carried as an opaque JSON string so the platform does not re-shape the backend&#39;s stream envelope.  | 

## Example

```python
from plexsphere.models.logs_query_result import LogsQueryResult

# TODO update the JSON string below
json = "{}"
# create an instance of LogsQueryResult from a JSON string
logs_query_result_instance = LogsQueryResult.from_json(json)
# print the JSON string representation of the object
print(LogsQueryResult.to_json())

# convert the object into a dict
logs_query_result_dict = logs_query_result_instance.to_dict()
# create an instance of LogsQueryResult from a dict
logs_query_result_from_dict = LogsQueryResult.from_dict(logs_query_result_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


