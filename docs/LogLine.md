# LogLine

A single structured log line in a `POST /v1/nodes/{id}/logs` batch. The batch body is newline-delimited JSON (`application/x-ndjson`), one `LogLine` object per line. Like every ingest record the line is tagged server-side with the originating Node's Domain and Project and routed to Grafana Loki; the platform never reads an identity subject or email off the line. `severity`, `message`, and `timestamp` are required; `unit` and `hostname` are optional. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**severity** | **str** | Syslog severity keyword of the log line. A value outside the closed set is rejected with &#x60;400 ingest_batch_malformed&#x60;.  | 
**unit** | **str** | Optional originating unit (the systemd unit or file-glob source) the line was collected from.  | [optional] 
**hostname** | **str** | Optional hostname the line was emitted on.  | [optional] 
**message** | **str** | The log message body. Required and non-empty. PII that a line inadvertently carries is the customer&#39;s ingest-side responsibility; an optional per-Domain redaction filter can be enabled at ingest.  | 
**timestamp** | **datetime** | RFC 3339 timestamp at which the line was emitted. Required.  | 

## Example

```python
from plexsphere.models.log_line import LogLine

# TODO update the JSON string below
json = "{}"
# create an instance of LogLine from a JSON string
log_line_instance = LogLine.from_json(json)
# print the JSON string representation of the object
print(LogLine.to_json())

# convert the object into a dict
log_line_dict = log_line_instance.to_dict()
# create an instance of LogLine from a dict
log_line_from_dict = LogLine.from_dict(log_line_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


