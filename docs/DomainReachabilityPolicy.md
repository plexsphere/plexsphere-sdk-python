# DomainReachabilityPolicy

Per-Domain heartbeat policy carried on Domain create / patch bodies and the Domain response. The three thresholds are linked: the aggregate rejects a partial fill (`invalid_reachability_policy`, 400) and replaces a fully-zero policy with the platform default.  DECISION: durations are encoded as Go-style strings (\"30s\", \"5m\") rather than seconds-int. The policy is operator-facing config — every other duration in the platform is rendered as a Go-style string in dashboards and CLI output, and the parser at the aggregate boundary already accepts the same syntax. The seconds-int alternative was rejected because it forced the dashboard to render a unit-less integer that operators would misread on first glance. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**heartbeat_interval** | **str** | Cadence agents must honour. Floor 10s, ceiling 1h. Encoded as a Go duration string (e.g. &#x60;30s&#x60;, &#x60;1m&#x60;).  | 
**stale_after** | **str** | Grace window before a Node is declared stale. Must be at least &#x60;3 × heartbeat_interval&#x60; and at most 1h.  | 
**unreachable_after** | **str** | Grace window before a Node is declared unreachable. Must be at least &#x60;2 × stale_after&#x60; and at most 1h.  | 

## Example

```python
from plexsphere.models.domain_reachability_policy import DomainReachabilityPolicy

# TODO update the JSON string below
json = "{}"
# create an instance of DomainReachabilityPolicy from a JSON string
domain_reachability_policy_instance = DomainReachabilityPolicy.from_json(json)
# print the JSON string representation of the object
print(DomainReachabilityPolicy.to_json())

# convert the object into a dict
domain_reachability_policy_dict = domain_reachability_policy_instance.to_dict()
# create an instance of DomainReachabilityPolicy from a dict
domain_reachability_policy_from_dict = DomainReachabilityPolicy.from_dict(domain_reachability_policy_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


