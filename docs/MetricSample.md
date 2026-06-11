# MetricSample

A single metric sample in a `POST /v1/nodes/{id}/metrics` batch. The batch body is a JSON array of `MetricSample` objects. Each sample is tagged server-side with the originating Node's Domain and Project — the wire body never carries an identity subject or email — so dashboards and alerts scope through the platform label model rather than through caller-supplied identity. `group`, `name`, `value`, and `timestamp` are required; `labels` is an optional dimension map. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**group** | **str** | The metric group the sample belongs to. The closed set (&#x60;node_resources&#x60;, &#x60;tunnel_health&#x60;, &#x60;peer_latency&#x60;, &#x60;agent_stats&#x60;) is the roster the ingest parser validates each record against; a value outside the set is rejected with &#x60;400 ingest_batch_malformed&#x60;.  | 
**name** | **str** | The metric name within its group (e.g. &#x60;cpu_seconds_total&#x60;). Required and non-empty.  | 
**value** | **float** | The numeric sample value. Required.  | 
**labels** | **Dict[str, str]** | Optional dimension map carried with the sample. Keys and values are caller-supplied label dimensions; the platform adds the Domain, Project, and Node tags itself and never reads an identity subject or email from this map.  | [optional] 
**timestamp** | **datetime** | RFC 3339 timestamp at which the sample was observed on the Node. Required.  | 

## Example

```python
from plexsphere.models.metric_sample import MetricSample

# TODO update the JSON string below
json = "{}"
# create an instance of MetricSample from a JSON string
metric_sample_instance = MetricSample.from_json(json)
# print the JSON string representation of the object
print(MetricSample.to_json())

# convert the object into a dict
metric_sample_dict = metric_sample_instance.to_dict()
# create an instance of MetricSample from a dict
metric_sample_from_dict = MetricSample.from_dict(metric_sample_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


