# plexsphere.CapacityApi

All URIs are relative to *http://localhost*

Method | HTTP request | Description
------------- | ------------- | -------------
[**get_domain_capacity**](CapacityApi.md#get_domain_capacity) | **GET** /v1/domains/{domainId}/capacity | Return the capacity-and-scale snapshot for a Domain.


# **get_domain_capacity**
> DomainCapacitySnapshot get_domain_capacity(domain_id)

Return the capacity-and-scale snapshot for a Domain.

Returns the latest per-Domain capacity snapshot the platform
collector samples on a fixed interval: one `used / target` reading
for each of the six catalogued capacity dimensions (`nodes`,
`sse_fanout`, `secret_reads`, `mediated_sessions`,
`observability_ingest`, `action_executions`). The Dashboard capacity
view and operators reading scale headroom consume this pull.

The handler:

  1. Authenticates the caller and rejects requests without a
     resolved Principal with 401.
  2. Checks the `domain-view` ReBAC relation on the addressed Domain
     BEFORE reading the snapshot so the endpoint cannot be used as a
     Domain-id oracle; an unauthorised caller receives 403.
  3. Returns the snapshot with 200 once the collector has sampled at
     least once. Before the first sample completes the snapshot is
     unavailable and the handler returns 503
     `capacity_snapshot_unavailable` with a `Retry-After` header.

DEFERRED-WIRING POSTURE: until the production composition root
supplies the collector-backed snapshot provider and the ReBAC
RelationChecker, every request returns 501 so log scrapers can alert
on the deferred-wiring state.


### Example


```python
import plexsphere
from plexsphere.models.domain_capacity_snapshot import DomainCapacitySnapshot
from plexsphere.rest import ApiException
from pprint import pprint

# Defining the host is optional and defaults to http://localhost
# See configuration.py for a list of all supported configuration parameters.
configuration = plexsphere.Configuration(
    host = "http://localhost"
)


# Enter a context with an instance of the API client
with plexsphere.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = plexsphere.CapacityApi(api_client)
    domain_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Domain. The Platform Audit Log is per-Domain by residency contract — cross-Domain decisions land in EACH affected Domain's chain, never on a shared system chain . Every audit endpoint requires the Domain identifier in the path. 

    try:
        # Return the capacity-and-scale snapshot for a Domain.
        api_response = api_instance.get_domain_capacity(domain_id)
        print("The response of CapacityApi->get_domain_capacity:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling CapacityApi->get_domain_capacity: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **domain_id** | **UUID**| Owning Domain. The Platform Audit Log is per-Domain by residency contract — cross-Domain decisions land in EACH affected Domain&#39;s chain, never on a shared system chain . Every audit endpoint requires the Domain identifier in the path.  | 

### Return type

[**DomainCapacitySnapshot**](DomainCapacitySnapshot.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Capacity snapshot for the addressed Domain.  |  -  |
**400** | The path &#x60;{domainId}&#x60; was not a non-zero UUID. The Problem body&#39;s &#x60;code&#x60; field is &#x60;invalid_domain_id&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the &#x60;domain-view&#x60; ReBAC relation on the addressed Domain. Surfaced before the snapshot read so the endpoint cannot be used as a Domain-id oracle.  |  -  |
**500** | Internal server error: a snapshot-read fault or an authorization-check fault surfaces here.  |  -  |
**501** | The capacity snapshot surface is not yet provisioned in this build, so log scrapers can alert on the deferred-wiring state. Resolves once the collector-backed snapshot provider and the ReBAC RelationChecker are wired into the v1 handler dependency cone.  |  -  |
**503** | The collector has not produced a snapshot for the Domain yet (no sample has completed since boot). The Problem body&#39;s &#x60;code&#x60; field is &#x60;capacity_snapshot_unavailable&#x60;, and the &#x60;Retry-After&#x60; response header carries the number of seconds the caller should wait before retrying.  |  * Retry-After - Seconds the caller should wait before retrying, derived from the collector&#39;s sample interval.  <br>  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

