# MetricsQueryResult

Result of a metrics query, projected from the metrics backend's response. `status_code` is the backend's HTTP status and `body` is the backend's verbatim JSON payload so the Dashboard can render the series without the platform re-encoding them. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**status_code** | **int** | HTTP status the metrics backend returned. | 
**body** | **str** | The metrics backend&#39;s verbatim response body, carried as an opaque JSON string so the platform does not re-shape the backend&#39;s series envelope.  | 

## Example

```python
from plexsphere.models.metrics_query_result import MetricsQueryResult

# TODO update the JSON string below
json = "{}"
# create an instance of MetricsQueryResult from a JSON string
metrics_query_result_instance = MetricsQueryResult.from_json(json)
# print the JSON string representation of the object
print(MetricsQueryResult.to_json())

# convert the object into a dict
metrics_query_result_dict = metrics_query_result_instance.to_dict()
# create an instance of MetricsQueryResult from a dict
metrics_query_result_from_dict = MetricsQueryResult.from_dict(metrics_query_result_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


