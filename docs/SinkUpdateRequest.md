# SinkUpdateRequest

Body for `PUT /v1/sinks/{id}`. It is the full post-image: a mutable field absent from the body is cleared, not left untouched.  `id`, `domain_id` and `built_in` are immutable once the sink exists and are therefore absent from the body; a write that would change any of them is refused with `409 sink_conflict`.  `expected_updated_at` is the compare-and-swap token and is REQUIRED. Because the body replaces the whole sink, a client writing from a stale read silently restores every mutable field it never touched — including `tls.insecure_skip_verify`, `tls.ca_pem` and `credential`. Stating the `updated_at` the client read makes that a `409 sink_cas_conflict` instead. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**expected_updated_at** | **datetime** | The &#x60;updated_at&#x60; the client read the sink at. The write is applied only if the stored sink still carries it; a sink changed since that read is refused with &#x60;409 sink_cas_conflict&#x60; and nothing is written.  | 
**slug** | **str** | Kebab-case handle, unique among the Domain&#39;s sinks. Moving it onto a slug another sink of the Domain holds is refused with &#x60;409 sink_slug_taken&#x60;.  | 
**display_name** | **str** | Human-readable name the sink is listed under. | 
**sink_type** | [**TenantSinkType**](TenantSinkType.md) |  | 
**endpoint** | **str** | Address to deliver to, valid for the stated type. A host naming a literal address inside the platform&#39;s own network is refused.  | 
**tls** | [**SinkTLS**](SinkTLS.md) |  | [optional] 
**credential** | [**SinkCredentialInput**](SinkCredentialInput.md) |  | [optional] 
**settings** | [**SinkSettings**](SinkSettings.md) |  | [optional] 

## Example

```python
from plexsphere.models.sink_update_request import SinkUpdateRequest

# TODO update the JSON string below
json = "{}"
# create an instance of SinkUpdateRequest from a JSON string
sink_update_request_instance = SinkUpdateRequest.from_json(json)
# print the JSON string representation of the object
print(SinkUpdateRequest.to_json())

# convert the object into a dict
sink_update_request_dict = sink_update_request_instance.to_dict()
# create an instance of SinkUpdateRequest from a dict
sink_update_request_from_dict = SinkUpdateRequest.from_dict(sink_update_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


