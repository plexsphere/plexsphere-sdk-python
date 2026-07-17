# MetricsQueryResult

Result of a metrics query, projected from the metrics backend's response. This envelope is only ever returned on a platform `200`: the pass-through boundary maps a backend 2xx to this body, a backend 4xx to a platform `400`, and a backend 429/5xx or an unreachable/timed-out backend to a platform `502`/`504`. So `status_code` is always a backend 2xx here; a backend error never rides inside a `200`. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**status_code** | **int** | The backend&#39;s 2xx HTTP status (a non-2xx backend response is mapped to a platform 4xx/5xx and never reaches this body).  | 
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


