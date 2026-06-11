# EndpointRequest

Body for PUT /v1/nodes/{id}/endpoint. Carries a single NAT- observed transport endpoint the plexd agent saw the local Node arrive on, plus the agent's classification of the NAT traversal posture and the wall-clock at which the observation was made. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**endpoint** | **str** | Canonical &#x60;host:port&#x60; wire form of the observed transport endpoint. The host is an IPv4 dotted quad or an IPv6 address bracketed per RFC 5952 (&#x60;[2001:db8::1]:51820&#x60;); the port is in the RFC 6056 ephemeral range 1..65535. Loopback, link-local, and unspecified addresses are refused with 400 &#x60;endpoint_unparseable&#x60;.  | 
**nat_type** | **str** | Agent-classified NAT traversal posture. The handler forwards the value verbatim to the Peer aggregate; the controller correlates the value with the per-Domain bridge view in later stories. &#x60;unknown&#x60; is permitted for agents that have not yet completed STUN probing.  | 
**reported_at** | **datetime** | Agent wall-clock at the moment the observation was made. The handler rejects the request with 400 &#x60;endpoint_clock_skew&#x60; if the value drifts more than 60 seconds from server now.  | 

## Example

```python
from plexsphere.models.endpoint_request import EndpointRequest

# TODO update the JSON string below
json = "{}"
# create an instance of EndpointRequest from a JSON string
endpoint_request_instance = EndpointRequest.from_json(json)
# print the JSON string representation of the object
print(EndpointRequest.to_json())

# convert the object into a dict
endpoint_request_dict = endpoint_request_instance.to_dict()
# create an instance of EndpointRequest from a dict
endpoint_request_from_dict = EndpointRequest.from_dict(endpoint_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


