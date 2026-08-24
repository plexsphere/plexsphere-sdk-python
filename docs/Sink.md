# Sink

One destination a Domain's routing layer may deliver to, either tenant-declared or built in. The two shapes are the same thing to a Telemetry Route: a named destination it may target.  A built-in sink names a platform backend and carries no `domain_id`, no `endpoint`, no `credential` and no `tls`; its slug equals its type. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**id** | **UUID** | Stable identifier of the sink (UUIDv7). | 
**domain_id** | **UUID** | Domain that declared the sink. Absent on a built-in sink, which belongs to no Domain.  | [optional] 
**slug** | **str** | Kebab-case handle the sink is addressed by within its scope — unique per Domain for a tenant sink, globally for a built-in one.  | 
**display_name** | **str** | Human-readable name the sink is listed under. | 
**sink_type** | [**SinkType**](SinkType.md) |  | 
**built_in** | **bool** | Whether the sink is platform-provided. A built-in sink is reachable from a route without an enablement and is neither updatable nor deletable.  | 
**endpoint** | **str** | Address the sink delivers to — a URL or a host and port pair, depending on the type. Empty on a built-in sink, which the platform addresses through its own configuration.  | 
**tls** | [**SinkTLS**](SinkTLS.md) |  | [optional] 
**credential** | [**SinkCredential**](SinkCredential.md) |  | [optional] 
**settings** | [**SinkSettings**](SinkSettings.md) |  | [optional] 
**created_at** | **datetime** | RFC 3339 timestamp the sink was declared. | 
**updated_at** | **datetime** | RFC 3339 timestamp the sink was last changed. | 

## Example

```python
from plexsphere.models.sink import Sink

# TODO update the JSON string below
json = "{}"
# create an instance of Sink from a JSON string
sink_instance = Sink.from_json(json)
# print the JSON string representation of the object
print(Sink.to_json())

# convert the object into a dict
sink_dict = sink_instance.to_dict()
# create an instance of Sink from a dict
sink_from_dict = Sink.from_dict(sink_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


