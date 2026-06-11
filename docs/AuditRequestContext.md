# AuditRequestContext

Inbound request context recorded alongside the decision. Both fields are optional because the audit row may be emitted from a code path with no live HTTP request (e.g. a background reconciler). The IP is the network-level client IP after proxy unwrapping and is recorded for incident triage; the user agent is recorded verbatim and is not parsed. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**ip** | **str** | Client IP after proxy unwrapping (IPv4 or IPv6 textual form). | [optional] 
**user_agent** | **str** | Verbatim &#x60;User-Agent&#x60; header value. | [optional] 

## Example

```python
from plexsphere.models.audit_request_context import AuditRequestContext

# TODO update the JSON string below
json = "{}"
# create an instance of AuditRequestContext from a JSON string
audit_request_context_instance = AuditRequestContext.from_json(json)
# print the JSON string representation of the object
print(AuditRequestContext.to_json())

# convert the object into a dict
audit_request_context_dict = audit_request_context_instance.to_dict()
# create an instance of AuditRequestContext from a dict
audit_request_context_from_dict = AuditRequestContext.from_dict(audit_request_context_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


