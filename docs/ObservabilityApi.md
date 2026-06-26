# plexsphere.ObservabilityApi

All URIs are relative to *http://localhost*

Method | HTTP request | Description
------------- | ------------- | -------------
[**append_incident_event**](ObservabilityApi.md#append_incident_event) | **POST** /v1/domains/{domainId}/incidents/{incidentId}/events | Append an event to an incident&#39;s timeline.
[**create_alert_rule**](ObservabilityApi.md#create_alert_rule) | **POST** /v1/domains/{domainId}/alert-rules | Store a new alert rule for a Domain.
[**delete_alert_rule**](ObservabilityApi.md#delete_alert_rule) | **DELETE** /v1/domains/{domainId}/alert-rules/{alertRuleId} | Delete a stored alert rule.
[**get_alert_rule**](ObservabilityApi.md#get_alert_rule) | **GET** /v1/domains/{domainId}/alert-rules/{alertRuleId} | Read a single stored alert rule.
[**get_incident**](ObservabilityApi.md#get_incident) | **GET** /v1/domains/{domainId}/incidents/{incidentId} | Read a single incident with its ordered timeline.
[**list_alert_rules**](ObservabilityApi.md#list_alert_rules) | **GET** /v1/domains/{domainId}/alert-rules | List the stored alert rules for a Domain.
[**list_incidents**](ObservabilityApi.md#list_incidents) | **GET** /v1/domains/{domainId}/incidents | List the incidents for a Domain.
[**open_incident**](ObservabilityApi.md#open_incident) | **POST** /v1/domains/{domainId}/incidents | Open a new incident for a Domain.
[**query_domain_logs**](ObservabilityApi.md#query_domain_logs) | **GET** /v1/domains/{domainId}/logs/query | Run a read-only LogQL logs query for a Domain.
[**query_domain_metrics**](ObservabilityApi.md#query_domain_metrics) | **GET** /v1/domains/{domainId}/metrics/query | Run a read-only PromQL metrics query for a Domain.
[**resolve_incident**](ObservabilityApi.md#resolve_incident) | **POST** /v1/domains/{domainId}/incidents/{incidentId}:resolve | Resolve an open incident.
[**update_alert_rule**](ObservabilityApi.md#update_alert_rule) | **PATCH** /v1/domains/{domainId}/alert-rules/{alertRuleId} | Update mutable fields on a stored alert rule.


# **append_incident_event**
> TimelineEvent append_incident_event(domain_id, incident_id, timeline_event_append)

Append an event to an incident's timeline.

Appends one event to the append-only timeline of the incident
addressed by `{incidentId}`. Events may only be appended while the
incident is open; appending to a resolved incident is rejected with
409. The handler checks the `domain-edit` ReBAC relation on the
addressed Domain before the write, then persists the event and returns
the appended timeline event.


### Example


```python
import plexsphere
from plexsphere.models.timeline_event import TimelineEvent
from plexsphere.models.timeline_event_append import TimelineEventAppend
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
    api_instance = plexsphere.ObservabilityApi(api_client)
    domain_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Domain. The Platform Audit Log is per-Domain by residency contract — cross-Domain decisions land in EACH affected Domain's chain, never on a shared system chain . Every audit endpoint requires the Domain identifier in the path. 
    incident_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Incident identifier (UUIDv7). Bound on `/v1/domains/{domainId}/incidents/{incidentId}`, its `/events` sub-resource, and its `:resolve` sub-resource. 
    timeline_event_append = {"kind":"note","message":"Paged the on-call operator; investigating the upstream dependency."} # TimelineEventAppend | 

    try:
        # Append an event to an incident's timeline.
        api_response = api_instance.append_incident_event(domain_id, incident_id, timeline_event_append)
        print("The response of ObservabilityApi->append_incident_event:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling ObservabilityApi->append_incident_event: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **domain_id** | **UUID**| Owning Domain. The Platform Audit Log is per-Domain by residency contract — cross-Domain decisions land in EACH affected Domain&#39;s chain, never on a shared system chain . Every audit endpoint requires the Domain identifier in the path.  | 
 **incident_id** | **UUID**| Incident identifier (UUIDv7). Bound on &#x60;/v1/domains/{domainId}/incidents/{incidentId}&#x60;, its &#x60;/events&#x60; sub-resource, and its &#x60;:resolve&#x60; sub-resource.  | 
 **timeline_event_append** | [**TimelineEventAppend**](TimelineEventAppend.md)|  | 

### Return type

[**TimelineEvent**](TimelineEvent.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**201** | The event was appended. The body carries the appended timeline event.  |  -  |
**400** | The body failed validation. The Problem body&#39;s &#x60;code&#x60; field is &#x60;timeline_event_invalid&#x60; — an unknown kind or an empty or oversized message.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the &#x60;domain-edit&#x60; ReBAC relation on the addressed Domain.  |  -  |
**404** | No incident with the id exists in the Domain. The Problem body&#39;s &#x60;code&#x60; field is &#x60;incident_not_found&#x60;.  |  -  |
**409** | The incident is already resolved, so its timeline is frozen. The Problem body&#39;s &#x60;code&#x60; field is &#x60;incident_resolved&#x60;.  |  -  |
**500** | Internal server error. |  -  |
**501** | The incident surface is not yet provisioned in this build, so log scrapers can alert on the deferred-wiring state.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **create_alert_rule**
> AlertRule create_alert_rule(domain_id, alert_rule_create)

Store a new alert rule for a Domain.

Stores a new alert rule for the addressed Domain. The rule is stored,
managed configuration only: the platform records and serves it and
does not evaluate it in this phase. The handler checks the
`domain-edit` ReBAC relation on the addressed Domain before any write,
validates the body, then persists the rule. The rule name is unique
within a Domain; a duplicate name is rejected with 409.


### Example


```python
import plexsphere
from plexsphere.models.alert_rule import AlertRule
from plexsphere.models.alert_rule_create import AlertRuleCreate
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
    api_instance = plexsphere.ObservabilityApi(api_client)
    domain_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Domain. The Platform Audit Log is per-Domain by residency contract — cross-Domain decisions land in EACH affected Domain's chain, never on a shared system chain . Every audit endpoint requires the Domain identifier in the path. 
    alert_rule_create = {"name":"node-cpu-saturation","signal":"avg(rate(node_cpu_seconds_total[5m]))","comparator":"gt","threshold":0.9,"severity":"warning"} # AlertRuleCreate | 

    try:
        # Store a new alert rule for a Domain.
        api_response = api_instance.create_alert_rule(domain_id, alert_rule_create)
        print("The response of ObservabilityApi->create_alert_rule:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling ObservabilityApi->create_alert_rule: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **domain_id** | **UUID**| Owning Domain. The Platform Audit Log is per-Domain by residency contract — cross-Domain decisions land in EACH affected Domain&#39;s chain, never on a shared system chain . Every audit endpoint requires the Domain identifier in the path.  | 
 **alert_rule_create** | [**AlertRuleCreate**](AlertRuleCreate.md)|  | 

### Return type

[**AlertRule**](AlertRule.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**201** | The alert rule was stored. The body carries the stored rule.  |  -  |
**400** | The body failed validation. The Problem body&#39;s &#x60;code&#x60; field is &#x60;alert_rule_invalid&#x60; — an empty or oversized name or signal, a non-finite threshold, an unknown comparator, or an unknown severity.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the &#x60;domain-edit&#x60; ReBAC relation on the addressed Domain.  |  -  |
**404** | The addressed Domain does not exist. The Problem body&#39;s &#x60;code&#x60; field is &#x60;domain_not_found&#x60;.  |  -  |
**409** | An alert rule with the same name already exists in the Domain. The Problem body&#39;s &#x60;code&#x60; field is &#x60;alert_rule_name_conflict&#x60;.  |  -  |
**500** | Internal server error. |  -  |
**501** | The alert-rule surface is not yet provisioned in this build, so log scrapers can alert on the deferred-wiring state.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **delete_alert_rule**
> delete_alert_rule(domain_id, alert_rule_id)

Delete a stored alert rule.

Deletes the alert rule addressed by `{alertRuleId}` within the Domain.
The handler checks the `domain-edit` ReBAC relation on the addressed
Domain before the delete. Deleting an absent rule returns 404.


### Example


```python
import plexsphere
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
    api_instance = plexsphere.ObservabilityApi(api_client)
    domain_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Domain. The Platform Audit Log is per-Domain by residency contract — cross-Domain decisions land in EACH affected Domain's chain, never on a shared system chain . Every audit endpoint requires the Domain identifier in the path. 
    alert_rule_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Alert rule identifier (UUIDv7). Bound on `/v1/domains/{domainId}/alert-rules/{alertRuleId}` for the single-rule read, update, and delete. 

    try:
        # Delete a stored alert rule.
        api_instance.delete_alert_rule(domain_id, alert_rule_id)
    except Exception as e:
        print("Exception when calling ObservabilityApi->delete_alert_rule: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **domain_id** | **UUID**| Owning Domain. The Platform Audit Log is per-Domain by residency contract — cross-Domain decisions land in EACH affected Domain&#39;s chain, never on a shared system chain . Every audit endpoint requires the Domain identifier in the path.  | 
 **alert_rule_id** | **UUID**| Alert rule identifier (UUIDv7). Bound on &#x60;/v1/domains/{domainId}/alert-rules/{alertRuleId}&#x60; for the single-rule read, update, and delete.  | 

### Return type

void (empty response body)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**204** | The alert rule was deleted. No body. |  -  |
**400** | A malformed &#x60;{domainId}&#x60; or &#x60;{alertRuleId}&#x60;. The Problem body&#39;s &#x60;code&#x60; field is &#x60;invalid_domain_id&#x60; or &#x60;invalid_alert_rule_id&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the &#x60;domain-edit&#x60; ReBAC relation on the addressed Domain.  |  -  |
**404** | No alert rule with the id exists in the Domain. The Problem body&#39;s &#x60;code&#x60; field is &#x60;alert_rule_not_found&#x60;.  |  -  |
**500** | Internal server error. |  -  |
**501** | The alert-rule surface is not yet provisioned in this build, so log scrapers can alert on the deferred-wiring state.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **get_alert_rule**
> AlertRule get_alert_rule(domain_id, alert_rule_id)

Read a single stored alert rule.

Returns the alert rule addressed by `{alertRuleId}` within the Domain.
The read is gated by the `domain-view` ReBAC relation on the addressed
Domain. Alert rules are stored, managed configuration only and are not
evaluated in this phase.


### Example


```python
import plexsphere
from plexsphere.models.alert_rule import AlertRule
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
    api_instance = plexsphere.ObservabilityApi(api_client)
    domain_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Domain. The Platform Audit Log is per-Domain by residency contract — cross-Domain decisions land in EACH affected Domain's chain, never on a shared system chain . Every audit endpoint requires the Domain identifier in the path. 
    alert_rule_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Alert rule identifier (UUIDv7). Bound on `/v1/domains/{domainId}/alert-rules/{alertRuleId}` for the single-rule read, update, and delete. 

    try:
        # Read a single stored alert rule.
        api_response = api_instance.get_alert_rule(domain_id, alert_rule_id)
        print("The response of ObservabilityApi->get_alert_rule:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling ObservabilityApi->get_alert_rule: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **domain_id** | **UUID**| Owning Domain. The Platform Audit Log is per-Domain by residency contract — cross-Domain decisions land in EACH affected Domain&#39;s chain, never on a shared system chain . Every audit endpoint requires the Domain identifier in the path.  | 
 **alert_rule_id** | **UUID**| Alert rule identifier (UUIDv7). Bound on &#x60;/v1/domains/{domainId}/alert-rules/{alertRuleId}&#x60; for the single-rule read, update, and delete.  | 

### Return type

[**AlertRule**](AlertRule.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | The stored alert rule. |  -  |
**400** | A malformed &#x60;{domainId}&#x60; or &#x60;{alertRuleId}&#x60;. The Problem body&#39;s &#x60;code&#x60; field is &#x60;invalid_domain_id&#x60; or &#x60;invalid_alert_rule_id&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the &#x60;domain-view&#x60; ReBAC relation on the addressed Domain.  |  -  |
**404** | No alert rule with the id exists in the Domain. The Problem body&#39;s &#x60;code&#x60; field is &#x60;alert_rule_not_found&#x60;.  |  -  |
**500** | Internal server error. |  -  |
**501** | The alert-rule surface is not yet provisioned in this build, so log scrapers can alert on the deferred-wiring state.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **get_incident**
> Incident get_incident(domain_id, incident_id)

Read a single incident with its ordered timeline.

Returns the incident addressed by `{incidentId}` within the Domain,
together with its append-only timeline ordered by `occurred_at`
ascending. The read is gated by the `domain-view` ReBAC relation on
the addressed Domain.


### Example


```python
import plexsphere
from plexsphere.models.incident import Incident
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
    api_instance = plexsphere.ObservabilityApi(api_client)
    domain_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Domain. The Platform Audit Log is per-Domain by residency contract — cross-Domain decisions land in EACH affected Domain's chain, never on a shared system chain . Every audit endpoint requires the Domain identifier in the path. 
    incident_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Incident identifier (UUIDv7). Bound on `/v1/domains/{domainId}/incidents/{incidentId}`, its `/events` sub-resource, and its `:resolve` sub-resource. 

    try:
        # Read a single incident with its ordered timeline.
        api_response = api_instance.get_incident(domain_id, incident_id)
        print("The response of ObservabilityApi->get_incident:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling ObservabilityApi->get_incident: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **domain_id** | **UUID**| Owning Domain. The Platform Audit Log is per-Domain by residency contract — cross-Domain decisions land in EACH affected Domain&#39;s chain, never on a shared system chain . Every audit endpoint requires the Domain identifier in the path.  | 
 **incident_id** | **UUID**| Incident identifier (UUIDv7). Bound on &#x60;/v1/domains/{domainId}/incidents/{incidentId}&#x60;, its &#x60;/events&#x60; sub-resource, and its &#x60;:resolve&#x60; sub-resource.  | 

### Return type

[**Incident**](Incident.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | The incident with its ordered timeline. |  -  |
**400** | A malformed &#x60;{domainId}&#x60; or &#x60;{incidentId}&#x60;. The Problem body&#39;s &#x60;code&#x60; field is &#x60;invalid_domain_id&#x60; or &#x60;invalid_incident_id&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the &#x60;domain-view&#x60; ReBAC relation on the addressed Domain.  |  -  |
**404** | No incident with the id exists in the Domain. The Problem body&#39;s &#x60;code&#x60; field is &#x60;incident_not_found&#x60;.  |  -  |
**500** | Internal server error. |  -  |
**501** | The incident surface is not yet provisioned in this build, so log scrapers can alert on the deferred-wiring state.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **list_alert_rules**
> AlertRuleList list_alert_rules(domain_id, cursor=cursor, limit=limit)

List the stored alert rules for a Domain.

Returns a cursor-paginated page of the alert rules stored for the
addressed Domain. Alert rules are stored, managed configuration: the
platform records and serves them and does not evaluate them in this
phase. The read is gated by the `domain-view` ReBAC relation on the
addressed Domain, checked before the persistence read.


### Example


```python
import plexsphere
from plexsphere.models.alert_rule_list import AlertRuleList
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
    api_instance = plexsphere.ObservabilityApi(api_client)
    domain_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Domain. The Platform Audit Log is per-Domain by residency contract — cross-Domain decisions land in EACH affected Domain's chain, never on a shared system chain . Every audit endpoint requires the Domain identifier in the path. 
    cursor = 'cursor_example' # str | Opaque pagination cursor — value of `next_cursor` from the previous page, or unset to start at the head of the list.  (optional)
    limit = 50 # int | Maximum number of alert rules to return on this page. Clamped server-side at 200.  (optional) (default to 50)

    try:
        # List the stored alert rules for a Domain.
        api_response = api_instance.list_alert_rules(domain_id, cursor=cursor, limit=limit)
        print("The response of ObservabilityApi->list_alert_rules:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling ObservabilityApi->list_alert_rules: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **domain_id** | **UUID**| Owning Domain. The Platform Audit Log is per-Domain by residency contract — cross-Domain decisions land in EACH affected Domain&#39;s chain, never on a shared system chain . Every audit endpoint requires the Domain identifier in the path.  | 
 **cursor** | **str**| Opaque pagination cursor — value of &#x60;next_cursor&#x60; from the previous page, or unset to start at the head of the list.  | [optional] 
 **limit** | **int**| Maximum number of alert rules to return on this page. Clamped server-side at 200.  | [optional] [default to 50]

### Return type

[**AlertRuleList**](AlertRuleList.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Page of stored alert rules for the addressed Domain.  |  -  |
**400** | Invalid query parameter — a malformed cursor (&#x60;code: cursor_invalid&#x60;) or a malformed &#x60;{domainId}&#x60; (&#x60;code: invalid_domain_id&#x60;).  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the &#x60;domain-view&#x60; ReBAC relation on the addressed Domain.  |  -  |
**404** | The addressed Domain does not exist. The Problem body&#39;s &#x60;code&#x60; field is &#x60;domain_not_found&#x60;.  |  -  |
**500** | Internal server error. |  -  |
**501** | The alert-rule surface is not yet provisioned in this build, so log scrapers can alert on the deferred-wiring state.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **list_incidents**
> IncidentList list_incidents(domain_id, cursor=cursor, limit=limit, status=status)

List the incidents for a Domain.

Returns a cursor-paginated page of incident headers for the addressed
Domain. The list projection is header-only — it omits the per-incident
timeline, which is returned by the single-incident read. The read is
gated by the `domain-view` ReBAC relation on the addressed Domain.


### Example


```python
import plexsphere
from plexsphere.models.incident_list import IncidentList
from plexsphere.models.incident_status import IncidentStatus
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
    api_instance = plexsphere.ObservabilityApi(api_client)
    domain_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Domain. The Platform Audit Log is per-Domain by residency contract — cross-Domain decisions land in EACH affected Domain's chain, never on a shared system chain . Every audit endpoint requires the Domain identifier in the path. 
    cursor = 'cursor_example' # str | Opaque pagination cursor — value of `next_cursor` from the previous page, or unset to start at the head of the list.  (optional)
    limit = 50 # int | Maximum number of incidents to return on this page. Clamped server-side at 200.  (optional) (default to 50)
    status = plexsphere.IncidentStatus() # IncidentStatus | Optional status filter so the Dashboard can show only open or only resolved incidents.  (optional)

    try:
        # List the incidents for a Domain.
        api_response = api_instance.list_incidents(domain_id, cursor=cursor, limit=limit, status=status)
        print("The response of ObservabilityApi->list_incidents:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling ObservabilityApi->list_incidents: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **domain_id** | **UUID**| Owning Domain. The Platform Audit Log is per-Domain by residency contract — cross-Domain decisions land in EACH affected Domain&#39;s chain, never on a shared system chain . Every audit endpoint requires the Domain identifier in the path.  | 
 **cursor** | **str**| Opaque pagination cursor — value of &#x60;next_cursor&#x60; from the previous page, or unset to start at the head of the list.  | [optional] 
 **limit** | **int**| Maximum number of incidents to return on this page. Clamped server-side at 200.  | [optional] [default to 50]
 **status** | [**IncidentStatus**](.md)| Optional status filter so the Dashboard can show only open or only resolved incidents.  | [optional] 

### Return type

[**IncidentList**](IncidentList.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Page of incident headers for the addressed Domain. |  -  |
**400** | Invalid query parameter — a malformed cursor (&#x60;code: cursor_invalid&#x60;), an unknown &#x60;status&#x60; (&#x60;code: incident_status_invalid&#x60;), or a malformed &#x60;{domainId}&#x60; (&#x60;code: invalid_domain_id&#x60;).  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the &#x60;domain-view&#x60; ReBAC relation on the addressed Domain.  |  -  |
**404** | The addressed Domain does not exist. The Problem body&#39;s &#x60;code&#x60; field is &#x60;domain_not_found&#x60;.  |  -  |
**500** | Internal server error. |  -  |
**501** | The incident surface is not yet provisioned in this build, so log scrapers can alert on the deferred-wiring state.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **open_incident**
> Incident open_incident(domain_id, incident_open_request)

Open a new incident for a Domain.

Opens a new incident for the addressed Domain in the `open` state with
an empty timeline. The handler checks the `domain-edit` ReBAC relation
on the addressed Domain before the write, validates the body, then
persists the incident header.


### Example


```python
import plexsphere
from plexsphere.models.incident import Incident
from plexsphere.models.incident_open_request import IncidentOpenRequest
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
    api_instance = plexsphere.ObservabilityApi(api_client)
    domain_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Domain. The Platform Audit Log is per-Domain by residency contract — cross-Domain decisions land in EACH affected Domain's chain, never on a shared system chain . Every audit endpoint requires the Domain identifier in the path. 
    incident_open_request = {"title":"Elevated mediated-session error rate","severity":"critical"} # IncidentOpenRequest | 

    try:
        # Open a new incident for a Domain.
        api_response = api_instance.open_incident(domain_id, incident_open_request)
        print("The response of ObservabilityApi->open_incident:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling ObservabilityApi->open_incident: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **domain_id** | **UUID**| Owning Domain. The Platform Audit Log is per-Domain by residency contract — cross-Domain decisions land in EACH affected Domain&#39;s chain, never on a shared system chain . Every audit endpoint requires the Domain identifier in the path.  | 
 **incident_open_request** | [**IncidentOpenRequest**](IncidentOpenRequest.md)|  | 

### Return type

[**Incident**](Incident.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**201** | The incident was opened. The body carries the opened incident with its empty timeline.  |  -  |
**400** | The body failed validation. The Problem body&#39;s &#x60;code&#x60; field is &#x60;incident_invalid&#x60; — an empty or oversized title or an unknown severity.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the &#x60;domain-edit&#x60; ReBAC relation on the addressed Domain.  |  -  |
**404** | The addressed Domain does not exist. The Problem body&#39;s &#x60;code&#x60; field is &#x60;domain_not_found&#x60;.  |  -  |
**500** | Internal server error. |  -  |
**501** | The incident surface is not yet provisioned in this build, so log scrapers can alert on the deferred-wiring state.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **query_domain_logs**
> LogsQueryResult query_domain_logs(domain_id, query, start, end, limit=limit, direction=direction, node_id=node_id)

Run a read-only LogQL logs query for a Domain.

Runs a single read-only LogQL range query against the bundled logs
backend for the addressed Domain and returns the backend's verbatim
result. The query is always a range query bounded by `start` and
`end`; `limit` caps the number of returned lines and `direction`
chooses the scan order.

The handler authenticates the caller (401 when no Principal resolves),
checks the `domain-view` ReBAC relation on the addressed Domain BEFORE
touching the backend so the endpoint cannot be used as a Domain-id
oracle (403 on denial), validates the query bounds, then forwards to
the logs backend.

Query bounds are enforced at this boundary so one oversized query
cannot overwhelm the shared logs store: the expression is capped at
4096 characters, the window may span at most 31 days, and the line
result is capped server-side. A query that violates a bound is
rejected with 400 before any backend call.

# DECISION: 502 covers an unreachable / failing Loki upstream and 504
# covers an upstream that did not answer in time, mirroring the metrics
# query surface. 501 matches the capacity surface's deferred-wiring
# posture so log scrapers can alert until the logs backend is wired.


### Example


```python
import plexsphere
from plexsphere.models.logs_query_result import LogsQueryResult
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
    api_instance = plexsphere.ObservabilityApi(api_client)
    domain_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Domain. The Platform Audit Log is per-Domain by residency contract — cross-Domain decisions land in EACH affected Domain's chain, never on a shared system chain . Every audit endpoint requires the Domain identifier in the path. 
    query = 'query_example' # str | The LogQL expression to evaluate. Capped at 4096 characters; an empty or oversized expression is rejected with 400. 
    start = '2013-10-20T19:20:30+01:00' # datetime | Inclusive lower bound of the query window (RFC 3339). `end` must be after `start` and the window may span at most 31 days. 
    end = '2013-10-20T19:20:30+01:00' # datetime | Inclusive upper bound of the query window (RFC 3339), strictly after `start`. 
    limit = 100 # int | Maximum number of log lines to return. Clamped server-side to protect the logs store from an unbounded read.  (optional) (default to 100)
    direction = backward # str | Scan order: `backward` returns the newest lines first, `forward` the oldest first.  (optional) (default to backward)
    node_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Optional Node filter. When present the query is scoped to the named Node so the Dashboard can drill into a single Node's logs.  (optional)

    try:
        # Run a read-only LogQL logs query for a Domain.
        api_response = api_instance.query_domain_logs(domain_id, query, start, end, limit=limit, direction=direction, node_id=node_id)
        print("The response of ObservabilityApi->query_domain_logs:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling ObservabilityApi->query_domain_logs: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **domain_id** | **UUID**| Owning Domain. The Platform Audit Log is per-Domain by residency contract — cross-Domain decisions land in EACH affected Domain&#39;s chain, never on a shared system chain . Every audit endpoint requires the Domain identifier in the path.  | 
 **query** | **str**| The LogQL expression to evaluate. Capped at 4096 characters; an empty or oversized expression is rejected with 400.  | 
 **start** | **datetime**| Inclusive lower bound of the query window (RFC 3339). &#x60;end&#x60; must be after &#x60;start&#x60; and the window may span at most 31 days.  | 
 **end** | **datetime**| Inclusive upper bound of the query window (RFC 3339), strictly after &#x60;start&#x60;.  | 
 **limit** | **int**| Maximum number of log lines to return. Clamped server-side to protect the logs store from an unbounded read.  | [optional] [default to 100]
 **direction** | **str**| Scan order: &#x60;backward&#x60; returns the newest lines first, &#x60;forward&#x60; the oldest first.  | [optional] [default to backward]
 **node_id** | **UUID**| Optional Node filter. When present the query is scoped to the named Node so the Dashboard can drill into a single Node&#39;s logs.  | [optional] 

### Return type

[**LogsQueryResult**](LogsQueryResult.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Logs query result projected from the backend response.  |  -  |
**400** | Invalid query parameter — an empty or oversized expression (&#x60;code: query_invalid&#x60;), a malformed &#x60;{domainId}&#x60; (&#x60;code: invalid_domain_id&#x60;), or a window whose &#x60;end&#x60; is not after &#x60;start&#x60; or that spans more than the maximum (&#x60;code: query_range_invalid&#x60;).  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the &#x60;domain-view&#x60; ReBAC relation on the addressed Domain. Surfaced before the backend read so the endpoint cannot be used as a Domain-id oracle.  |  -  |
**404** | The addressed Domain does not exist. The Problem body&#39;s &#x60;code&#x60; field is &#x60;domain_not_found&#x60;.  |  -  |
**500** | Internal server error: a query-projection fault or an authorization-check fault surfaces here.  |  -  |
**501** | The logs query surface is not yet provisioned in this build, so log scrapers can alert on the deferred-wiring state. Resolves once the logs backend is wired into the v1 handler dependency cone.  |  -  |
**502** | The logs backend was unreachable or returned a backend error. The Problem body&#39;s &#x60;code&#x60; field is &#x60;query_upstream_unavailable&#x60;.  |  -  |
**504** | The logs backend did not answer within the query deadline. The Problem body&#39;s &#x60;code&#x60; field is &#x60;query_upstream_timeout&#x60;.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **query_domain_metrics**
> MetricsQueryResult query_domain_metrics(domain_id, query, time=time, start=start, end=end, step=step, node_id=node_id)

Run a read-only PromQL metrics query for a Domain.

Runs a single read-only PromQL query against the bundled metrics
backend for the addressed Domain and returns the backend's verbatim
result. Either an instant query (supply `time`) or a range query
(supply `start`, `end`, and `step`) is run; when `time` is absent the
handler runs a range query and requires the range bounds.

The handler authenticates the caller (401 when no Principal resolves),
checks the `domain-view` ReBAC relation on the addressed Domain BEFORE
touching the backend so the endpoint cannot be used as a Domain-id
oracle (403 on denial), validates the query bounds, then forwards to
the metrics backend.

Query bounds are enforced at this boundary so one oversized query
cannot overwhelm the shared metrics store: the expression is capped at
4096 characters, a range window may span at most 31 days, and the
series result is capped server-side. A query that violates a bound is
rejected with 400 before any backend call.

# DECISION: 502 covers an unreachable / failing Mimir upstream and 504
# covers an upstream that did not answer in time, so the operator can
# tell an unavailable backend apart from a slow one. 501 matches the
# capacity surface's deferred-wiring posture: until the production
# composition root supplies the query backends, every request returns
# 501 so log scrapers can alert on the deferred-wiring state.


### Example


```python
import plexsphere
from plexsphere.models.metrics_query_result import MetricsQueryResult
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
    api_instance = plexsphere.ObservabilityApi(api_client)
    domain_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Domain. The Platform Audit Log is per-Domain by residency contract — cross-Domain decisions land in EACH affected Domain's chain, never on a shared system chain . Every audit endpoint requires the Domain identifier in the path. 
    query = 'query_example' # str | The PromQL expression to evaluate. Capped at 4096 characters; an empty or oversized expression is rejected with 400. 
    time = '2013-10-20T19:20:30+01:00' # datetime | Evaluation instant for an instant query (RFC 3339). When present the handler runs an instant query and ignores `start` / `end` / `step`; when absent the handler runs a range query and requires `start`, `end`, and `step`.  (optional)
    start = '2013-10-20T19:20:30+01:00' # datetime | Inclusive lower bound of the range-query window (RFC 3339). Required for a range query; `end` must be after `start` and the window may span at most 31 days.  (optional)
    end = '2013-10-20T19:20:30+01:00' # datetime | Inclusive upper bound of the range-query window (RFC 3339). Required for a range query and must be after `start`.  (optional)
    step = 'step_example' # str | Range-query resolution step expressed as a duration (for example `30s`, `5m`). Required for a range query and ignored for an instant query.  (optional)
    node_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Optional Node filter. When present the query is scoped to the named Node so the Dashboard can drill into a single Node's metrics.  (optional)

    try:
        # Run a read-only PromQL metrics query for a Domain.
        api_response = api_instance.query_domain_metrics(domain_id, query, time=time, start=start, end=end, step=step, node_id=node_id)
        print("The response of ObservabilityApi->query_domain_metrics:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling ObservabilityApi->query_domain_metrics: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **domain_id** | **UUID**| Owning Domain. The Platform Audit Log is per-Domain by residency contract — cross-Domain decisions land in EACH affected Domain&#39;s chain, never on a shared system chain . Every audit endpoint requires the Domain identifier in the path.  | 
 **query** | **str**| The PromQL expression to evaluate. Capped at 4096 characters; an empty or oversized expression is rejected with 400.  | 
 **time** | **datetime**| Evaluation instant for an instant query (RFC 3339). When present the handler runs an instant query and ignores &#x60;start&#x60; / &#x60;end&#x60; / &#x60;step&#x60;; when absent the handler runs a range query and requires &#x60;start&#x60;, &#x60;end&#x60;, and &#x60;step&#x60;.  | [optional] 
 **start** | **datetime**| Inclusive lower bound of the range-query window (RFC 3339). Required for a range query; &#x60;end&#x60; must be after &#x60;start&#x60; and the window may span at most 31 days.  | [optional] 
 **end** | **datetime**| Inclusive upper bound of the range-query window (RFC 3339). Required for a range query and must be after &#x60;start&#x60;.  | [optional] 
 **step** | **str**| Range-query resolution step expressed as a duration (for example &#x60;30s&#x60;, &#x60;5m&#x60;). Required for a range query and ignored for an instant query.  | [optional] 
 **node_id** | **UUID**| Optional Node filter. When present the query is scoped to the named Node so the Dashboard can drill into a single Node&#39;s metrics.  | [optional] 

### Return type

[**MetricsQueryResult**](MetricsQueryResult.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Metrics query result projected from the backend response.  |  -  |
**400** | Invalid query parameter — an empty or oversized expression (&#x60;code: query_invalid&#x60;), a malformed &#x60;{domainId}&#x60; (&#x60;code: invalid_domain_id&#x60;), or a range window whose &#x60;end&#x60; is not after &#x60;start&#x60; or that spans more than the maximum (&#x60;code: query_range_invalid&#x60;).  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the &#x60;domain-view&#x60; ReBAC relation on the addressed Domain. Surfaced before the backend read so the endpoint cannot be used as a Domain-id oracle.  |  -  |
**404** | The addressed Domain does not exist. The Problem body&#39;s &#x60;code&#x60; field is &#x60;domain_not_found&#x60;.  |  -  |
**500** | Internal server error: a query-projection fault or an authorization-check fault surfaces here.  |  -  |
**501** | The metrics query surface is not yet provisioned in this build, so log scrapers can alert on the deferred-wiring state. Resolves once the metrics backend is wired into the v1 handler dependency cone.  |  -  |
**502** | The metrics backend was unreachable or returned a backend error. The Problem body&#39;s &#x60;code&#x60; field is &#x60;query_upstream_unavailable&#x60;.  |  -  |
**504** | The metrics backend did not answer within the query deadline. The Problem body&#39;s &#x60;code&#x60; field is &#x60;query_upstream_timeout&#x60;.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **resolve_incident**
> Incident resolve_incident(domain_id, incident_id)

Resolve an open incident.

Resolves the open incident addressed by `{incidentId}`, transitioning
it to the `resolved` state and stamping its resolved-at timestamp. The
handler checks the `domain-edit` ReBAC relation on the addressed
Domain before the write. Resolving an incident that is already resolved
is rejected with 409 — the lifecycle permits a single resolve.


### Example


```python
import plexsphere
from plexsphere.models.incident import Incident
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
    api_instance = plexsphere.ObservabilityApi(api_client)
    domain_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Domain. The Platform Audit Log is per-Domain by residency contract — cross-Domain decisions land in EACH affected Domain's chain, never on a shared system chain . Every audit endpoint requires the Domain identifier in the path. 
    incident_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Incident identifier (UUIDv7). Bound on `/v1/domains/{domainId}/incidents/{incidentId}`, its `/events` sub-resource, and its `:resolve` sub-resource. 

    try:
        # Resolve an open incident.
        api_response = api_instance.resolve_incident(domain_id, incident_id)
        print("The response of ObservabilityApi->resolve_incident:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling ObservabilityApi->resolve_incident: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **domain_id** | **UUID**| Owning Domain. The Platform Audit Log is per-Domain by residency contract — cross-Domain decisions land in EACH affected Domain&#39;s chain, never on a shared system chain . Every audit endpoint requires the Domain identifier in the path.  | 
 **incident_id** | **UUID**| Incident identifier (UUIDv7). Bound on &#x60;/v1/domains/{domainId}/incidents/{incidentId}&#x60;, its &#x60;/events&#x60; sub-resource, and its &#x60;:resolve&#x60; sub-resource.  | 

### Return type

[**Incident**](Incident.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | The incident was resolved. The body carries the updated incident.  |  -  |
**400** | A malformed &#x60;{domainId}&#x60; or &#x60;{incidentId}&#x60;. The Problem body&#39;s &#x60;code&#x60; field is &#x60;invalid_domain_id&#x60; or &#x60;invalid_incident_id&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the &#x60;domain-edit&#x60; ReBAC relation on the addressed Domain.  |  -  |
**404** | No incident with the id exists in the Domain. The Problem body&#39;s &#x60;code&#x60; field is &#x60;incident_not_found&#x60;.  |  -  |
**409** | The incident is already resolved. The Problem body&#39;s &#x60;code&#x60; field is &#x60;incident_already_resolved&#x60;.  |  -  |
**500** | Internal server error. |  -  |
**501** | The incident surface is not yet provisioned in this build, so log scrapers can alert on the deferred-wiring state.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **update_alert_rule**
> AlertRule update_alert_rule(domain_id, alert_rule_id, alert_rule_update)

Update mutable fields on a stored alert rule.

Applies a partial update to the alert rule addressed by
`{alertRuleId}`. Every body field is optional: an absent field leaves
the stored value untouched. The rule remains stored, managed
configuration only and is not evaluated in this phase. The handler
checks the `domain-edit` ReBAC relation on the addressed Domain before
any write, applies the patch, and persists the result. A rename onto
an existing name in the Domain is rejected with 409.


### Example


```python
import plexsphere
from plexsphere.models.alert_rule import AlertRule
from plexsphere.models.alert_rule_update import AlertRuleUpdate
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
    api_instance = plexsphere.ObservabilityApi(api_client)
    domain_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Domain. The Platform Audit Log is per-Domain by residency contract — cross-Domain decisions land in EACH affected Domain's chain, never on a shared system chain . Every audit endpoint requires the Domain identifier in the path. 
    alert_rule_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Alert rule identifier (UUIDv7). Bound on `/v1/domains/{domainId}/alert-rules/{alertRuleId}` for the single-rule read, update, and delete. 
    alert_rule_update = {"threshold":0.95,"severity":"critical","enabled":false} # AlertRuleUpdate | 

    try:
        # Update mutable fields on a stored alert rule.
        api_response = api_instance.update_alert_rule(domain_id, alert_rule_id, alert_rule_update)
        print("The response of ObservabilityApi->update_alert_rule:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling ObservabilityApi->update_alert_rule: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **domain_id** | **UUID**| Owning Domain. The Platform Audit Log is per-Domain by residency contract — cross-Domain decisions land in EACH affected Domain&#39;s chain, never on a shared system chain . Every audit endpoint requires the Domain identifier in the path.  | 
 **alert_rule_id** | **UUID**| Alert rule identifier (UUIDv7). Bound on &#x60;/v1/domains/{domainId}/alert-rules/{alertRuleId}&#x60; for the single-rule read, update, and delete.  | 
 **alert_rule_update** | [**AlertRuleUpdate**](AlertRuleUpdate.md)|  | 

### Return type

[**AlertRule**](AlertRule.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | The updated alert rule. |  -  |
**400** | A malformed id or a body that failed validation. The Problem body&#39;s &#x60;code&#x60; field is &#x60;invalid_alert_rule_id&#x60; or &#x60;alert_rule_invalid&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the &#x60;domain-edit&#x60; ReBAC relation on the addressed Domain.  |  -  |
**404** | No alert rule with the id exists in the Domain. The Problem body&#39;s &#x60;code&#x60; field is &#x60;alert_rule_not_found&#x60;.  |  -  |
**409** | A rename would collide with an existing rule name in the Domain. The Problem body&#39;s &#x60;code&#x60; field is &#x60;alert_rule_name_conflict&#x60;.  |  -  |
**500** | Internal server error. |  -  |
**501** | The alert-rule surface is not yet provisioned in this build, so log scrapers can alert on the deferred-wiring state.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

