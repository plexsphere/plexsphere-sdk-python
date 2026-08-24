# SinkCreateRequest

Body for `POST /v1/domains/{id}/sinks`. Declares one tenant sink for the addressed Domain. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**id** | **UUID** | Identifier to declare the sink under (UUIDv7). Absent lets the server mint one. A caller that stores connection material mints the id first so it can derive the KV path the material is written to before the sink exists.  | [optional] 
**slug** | **str** | Kebab-case handle, unique among the Domain&#39;s sinks. Leading and trailing whitespace is refused rather than trimmed.  | 
**display_name** | **str** | Human-readable name the sink is listed under. | 
**sink_type** | [**TenantSinkType**](TenantSinkType.md) |  | 
**endpoint** | **str** | Address to deliver to: an &#x60;https&#x60; URL for &#x60;otlp&#x60;, a host and port pair for &#x60;syslog&#x60;. A host naming a literal address inside the platform&#39;s own network is refused.  | 
**tls** | [**SinkTLS**](SinkTLS.md) |  | [optional] 
**credential** | [**SinkCredentialInput**](SinkCredentialInput.md) |  | [optional] 
**settings** | [**SinkSettings**](SinkSettings.md) |  | [optional] 

## Example

```python
from plexsphere.models.sink_create_request import SinkCreateRequest

# TODO update the JSON string below
json = "{}"
# create an instance of SinkCreateRequest from a JSON string
sink_create_request_instance = SinkCreateRequest.from_json(json)
# print the JSON string representation of the object
print(SinkCreateRequest.to_json())

# convert the object into a dict
sink_create_request_dict = sink_create_request_instance.to_dict()
# create an instance of SinkCreateRequest from a dict
sink_create_request_from_dict = SinkCreateRequest.from_dict(sink_create_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


