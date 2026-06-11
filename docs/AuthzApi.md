# plexsphere.AuthzApi

All URIs are relative to *http://localhost*

Method | HTTP request | Description
------------- | ------------- | -------------
[**create_relation_tuple**](AuthzApi.md#create_relation_tuple) | **POST** /v1/authz/relation-tuples | Create a relation tuple under a Project.
[**delete_relation_tuple**](AuthzApi.md#delete_relation_tuple) | **DELETE** /v1/authz/relation-tuples/{id} | Delete a relation tuple.
[**list_relation_tuples**](AuthzApi.md#list_relation_tuples) | **GET** /v1/authz/relation-tuples | List relation tuples scoped to a Project.
[**patch_relation_tuple**](AuthzApi.md#patch_relation_tuple) | **PATCH** /v1/authz/relation-tuples/{id} | Replace a relation tuple&#39;s contents (delete-then-write).
[**post_authz_check**](AuthzApi.md#post_authz_check) | **POST** /v1/authz/check | Run a ReBAC permission check.
[**post_authz_lookup_resources**](AuthzApi.md#post_authz_lookup_resources) | **POST** /v1/authz/lookup-resources | List the resources a subject can reach via a relation.
[**post_authz_lookup_subjects**](AuthzApi.md#post_authz_lookup_subjects) | **POST** /v1/authz/lookup-subjects | List the subjects that can reach a resource via a relation.


# **create_relation_tuple**
> RelationTuple create_relation_tuple(project_id, relation_tuple_create_request)

Create a relation tuple under a Project.

Persists a new relation tuple `(subject, relation, resource)`
scoped to the addressed Project (selected by the required
`project_id` query parameter). The handler authorises `manage`
on `project:<id>` BEFORE invoking the service so an
unauthorised caller never produces a `RelationTupleCreated`
outbox row.

The optional `caveat_context` is the set of caveat field
NAMES the tuple carries — never values. Carrying values across
the contract boundary would leak request-time data into
persisted ReBAC state; the audit row records the same NAMES-
only projection so the cross-boundary invariant survives
through the projection layer.


### Example


```python
import plexsphere
from plexsphere.models.relation_tuple import RelationTuple
from plexsphere.models.relation_tuple_create_request import RelationTupleCreateRequest
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
    api_instance = plexsphere.AuthzApi(api_client)
    project_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Project (UUIDv7).
    relation_tuple_create_request = {"subject":"user:0190a8b8-a0c0-7a0a-8a0a-a0a0a0a0a0a1","relation":"maintainer","resource":"project:0190a8b8-a0c0-7a0a-8a0a-a0a0a0a0a0aa"} # RelationTupleCreateRequest | 

    try:
        # Create a relation tuple under a Project.
        api_response = api_instance.create_relation_tuple(project_id, relation_tuple_create_request)
        print("The response of AuthzApi->create_relation_tuple:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling AuthzApi->create_relation_tuple: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **project_id** | **UUID**| Owning Project (UUIDv7). | 
 **relation_tuple_create_request** | [**RelationTupleCreateRequest**](RelationTupleCreateRequest.md)|  | 

### Return type

[**RelationTuple**](RelationTuple.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**201** | Relation tuple created. |  * Location - Absolute or root-relative URL of the persisted tuple (e.g. &#x60;/v1/authz/relation-tuples/&lt;id&gt;&#x60;).  <br>  |
**400** | Aggregate invariant rejected the body — empty subject / relation / resource, malformed object reference, etc. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;invalid_relation_tuple&#x60;, &#x60;invalid_body&#x60; }.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to create relation tuples on the addressed Project. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Owning Project does not exist or is not visible to the caller. Body is a &#x60;Problem&#x60; with &#x60;code: project_not_found&#x60;.  |  -  |
**413** | Request body exceeded the 8 KiB ceiling enforced by the handler.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **delete_relation_tuple**
> delete_relation_tuple(id)

Delete a relation tuple.

Deletes the relation tuple identified by `{id}`. The
operation is idempotent — a `{id}` that was already deleted
surfaces as `404 relation_tuple_not_found` rather than `204`,
so a caller racing two deletes can distinguish "I deleted it"
from "someone else got there first" without leaking existence
side-channels (the read path runs the same authz check before
the lookup).


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
    api_instance = plexsphere.AuthzApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Relation tuple identifier (UUIDv7). Bound on `/v1/authz/relation-tuples/{id}` for the per-tuple PATCH / DELETE surface. 

    try:
        # Delete a relation tuple.
        api_instance.delete_relation_tuple(id)
    except Exception as e:
        print("Exception when calling AuthzApi->delete_relation_tuple: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Relation tuple identifier (UUIDv7). Bound on &#x60;/v1/authz/relation-tuples/{id}&#x60; for the per-tuple PATCH / DELETE surface.  | 

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
**204** | Relation tuple deleted. |  -  |
**400** | Path &#x60;{id}&#x60; was not a non-zero UUID. Body is a &#x60;Problem&#x60; with &#x60;code: invalid_relation_tuple_id&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to delete the addressed relation tuple. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Relation tuple not found. Body is a &#x60;Problem&#x60; with &#x60;code: relation_tuple_not_found&#x60;.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **list_relation_tuples**
> RelationTupleList list_relation_tuples(project_id, cursor=cursor, limit=limit)

List relation tuples scoped to a Project.

Returns a page of relation tuples persisted under the addressed
Project (selected by the required `project_id` query
parameter). Per-row visibility is layered on top of the page —
rows the caller cannot `read` are filtered out so the response
items are a subset of the persistence-level page. Cursor
pagination mirrors `/v1/projects` exactly.


### Example


```python
import plexsphere
from plexsphere.models.relation_tuple_list import RelationTupleList
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
    api_instance = plexsphere.AuthzApi(api_client)
    project_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Project (UUIDv7). Relation-tuple administration is scoped per Project so every list call resolves to a single Project's tuples. 
    cursor = 'cursor_example' # str | Opaque continuation token returned by a previous call's `next_cursor`. The encoding is HMAC-signed by the server so a tampered cursor surfaces as `400`.  (optional)
    limit = 50 # int | Maximum number of items to return in a single page. The handler clamps the value to [1, 200] before forwarding it to the service.  (optional) (default to 50)

    try:
        # List relation tuples scoped to a Project.
        api_response = api_instance.list_relation_tuples(project_id, cursor=cursor, limit=limit)
        print("The response of AuthzApi->list_relation_tuples:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling AuthzApi->list_relation_tuples: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **project_id** | **UUID**| Owning Project (UUIDv7). Relation-tuple administration is scoped per Project so every list call resolves to a single Project&#39;s tuples.  | 
 **cursor** | **str**| Opaque continuation token returned by a previous call&#39;s &#x60;next_cursor&#x60;. The encoding is HMAC-signed by the server so a tampered cursor surfaces as &#x60;400&#x60;.  | [optional] 
 **limit** | **int**| Maximum number of items to return in a single page. The handler clamps the value to [1, 200] before forwarding it to the service.  | [optional] [default to 50]

### Return type

[**RelationTupleList**](RelationTupleList.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Page of relation tuples. |  -  |
**400** | Invalid query parameters — typically a missing or malformed &#x60;project_id&#x60;, a tampered cursor, or an out-of-range limit.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to list relation tuples on the addressed Project. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **patch_relation_tuple**
> RelationTuple patch_relation_tuple(id, relation_tuple_patch_request)

Replace a relation tuple's contents (delete-then-write).

Replaces the relation tuple identified by `{id}`. The
handler implements Delete-then-Write semantics — the existing
tuple is deleted and a new tuple persisted in the same
transaction so cursor-paginated readers never observe a
half-applied state.


### Example


```python
import plexsphere
from plexsphere.models.relation_tuple import RelationTuple
from plexsphere.models.relation_tuple_patch_request import RelationTuplePatchRequest
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
    api_instance = plexsphere.AuthzApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Relation tuple identifier (UUIDv7). Bound on `/v1/authz/relation-tuples/{id}` for the per-tuple PATCH / DELETE surface. 
    relation_tuple_patch_request = plexsphere.RelationTuplePatchRequest() # RelationTuplePatchRequest | 

    try:
        # Replace a relation tuple's contents (delete-then-write).
        api_response = api_instance.patch_relation_tuple(id, relation_tuple_patch_request)
        print("The response of AuthzApi->patch_relation_tuple:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling AuthzApi->patch_relation_tuple: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Relation tuple identifier (UUIDv7). Bound on &#x60;/v1/authz/relation-tuples/{id}&#x60; for the per-tuple PATCH / DELETE surface.  | 
 **relation_tuple_patch_request** | [**RelationTuplePatchRequest**](RelationTuplePatchRequest.md)|  | 

### Return type

[**RelationTuple**](RelationTuple.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Relation tuple replaced. |  -  |
**400** | Path &#x60;{id}&#x60; was not a non-zero UUID, body was empty, or the new tuple invariants were rejected. Body is a &#x60;Problem&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to replace the addressed relation tuple. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Relation tuple not found. Body is a &#x60;Problem&#x60; with &#x60;code: relation_tuple_not_found&#x60;.  |  -  |
**413** | Request body exceeded the 8 KiB ceiling enforced by the handler.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **post_authz_check**
> RebacCheckResponse post_authz_check(rebac_check_request)

Run a ReBAC permission check.

Evaluates the ReBAC tuple graph and (optional) caveat program
for the addressed (`subject`, `relation`, `resource`) triple
and returns the decision as data on `200`.

DECISION: a denial surfaces as `200 { decision: denied, reason
}`, NEVER as `403`. The HTTP layer reserves `403` for transport
denials (the caller is not even authorised to USE the check
endpoint). Mixing the two would force callers to disambiguate
infrastructure failures from policy outcomes by parsing the
Problem body, defeating the point of treating denials as a
first-class result.

The `correlation_id` echoed on every response pairs the result
with the matching audit entry emitted by `internal/audit`.


### Example


```python
import plexsphere
from plexsphere.models.rebac_check_request import RebacCheckRequest
from plexsphere.models.rebac_check_response import RebacCheckResponse
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
    api_instance = plexsphere.AuthzApi(api_client)
    rebac_check_request = {"subject":"user:0190a8b8-a0c0-7a0a-8a0a-a0a0a0a0a0a1","relation":"read","resource":"project:0190a8b8-a0c0-7a0a-8a0a-a0a0a0a0a0aa"} # RebacCheckRequest | 

    try:
        # Run a ReBAC permission check.
        api_response = api_instance.post_authz_check(rebac_check_request)
        print("The response of AuthzApi->post_authz_check:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling AuthzApi->post_authz_check: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **rebac_check_request** | [**RebacCheckRequest**](RebacCheckRequest.md)|  | 

### Return type

[**RebacCheckResponse**](RebacCheckResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | ReBAC decision. The body always carries &#x60;decision&#x60; and &#x60;correlation_id&#x60;. &#x60;decision: allowed&#x60; additionally carries &#x60;relation_path&#x60;; &#x60;decision: denied&#x60; carries &#x60;reason&#x60;.  |  -  |
**400** | Body could not be decoded or carried a malformed subject / relation / resource. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;invalid_body&#x60;, &#x60;invalid_check_request&#x60; }.  |  -  |
**401** | Caller is not authenticated. |  -  |
**413** | Request body exceeded the 8 KiB ceiling enforced by the handler.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **post_authz_lookup_resources**
> RebacLookupResponse post_authz_lookup_resources(lookup_resources_request)

List the resources a subject can reach via a relation.

Enumerates every resource of `resource_type` on which the
addressed `subject` holds `relation`, evaluating the ReBAC
tuple graph and the optional caveat program.

DECISION: the enumeration is materialised data on `200`, like
`/v1/authz/check`. An empty result is a normal answer, NEVER
an HTTP error, and there is no `403` — mirroring the Check
endpoint's posture that ReBAC outcomes are first-class data,
not transport failures.

The `correlation_id` echoed on the response pairs the result
with the matching audit entry emitted by `internal/audit`.


### Example


```python
import plexsphere
from plexsphere.models.lookup_resources_request import LookupResourcesRequest
from plexsphere.models.rebac_lookup_response import RebacLookupResponse
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
    api_instance = plexsphere.AuthzApi(api_client)
    lookup_resources_request = {"subject":"user:0190a8b8-a0c0-7a0a-8a0a-a0a0a0a0a0a1","relation":"read","resource_type":"project"} # LookupResourcesRequest | 

    try:
        # List the resources a subject can reach via a relation.
        api_response = api_instance.post_authz_lookup_resources(lookup_resources_request)
        print("The response of AuthzApi->post_authz_lookup_resources:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling AuthzApi->post_authz_lookup_resources: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **lookup_resources_request** | [**LookupResourcesRequest**](LookupResourcesRequest.md)|  | 

### Return type

[**RebacLookupResponse**](RebacLookupResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Materialised list of resource object ids the subject can reach. &#x60;items&#x60; is the full result set; an empty array is a normal answer.  |  -  |
**400** | Body could not be decoded or carried a malformed subject / relation / resource_type. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;invalid_body&#x60;, &#x60;invalid_lookup_request&#x60; }.  |  -  |
**401** | Caller is not authenticated. |  -  |
**413** | Request body exceeded the 8 KiB ceiling enforced by the handler.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **post_authz_lookup_subjects**
> RebacLookupResponse post_authz_lookup_subjects(lookup_subjects_request)

List the subjects that can reach a resource via a relation.

The dual of `/v1/authz/lookup-resources`: enumerates every
subject of `subject_type` that holds `relation` on the
addressed `resource`, evaluating the ReBAC tuple graph and the
optional caveat program.

DECISION: identical posture to `/v1/authz/lookup-resources`
and `/v1/authz/check` — the enumeration is materialised data
on `200`, an empty result is a normal answer, and there is no
`403`.

The `correlation_id` echoed on the response pairs the result
with the matching audit entry emitted by `internal/audit`.


### Example


```python
import plexsphere
from plexsphere.models.lookup_subjects_request import LookupSubjectsRequest
from plexsphere.models.rebac_lookup_response import RebacLookupResponse
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
    api_instance = plexsphere.AuthzApi(api_client)
    lookup_subjects_request = {"subject_type":"user","relation":"read","resource":"project:0190a8b8-a0c0-7a0a-8a0a-a0a0a0a0a0aa"} # LookupSubjectsRequest | 

    try:
        # List the subjects that can reach a resource via a relation.
        api_response = api_instance.post_authz_lookup_subjects(lookup_subjects_request)
        print("The response of AuthzApi->post_authz_lookup_subjects:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling AuthzApi->post_authz_lookup_subjects: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **lookup_subjects_request** | [**LookupSubjectsRequest**](LookupSubjectsRequest.md)|  | 

### Return type

[**RebacLookupResponse**](RebacLookupResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Materialised list of subject object ids that can reach the resource. &#x60;items&#x60; is the full result set; an empty array is a normal answer.  |  -  |
**400** | Body could not be decoded or carried a malformed subject_type / relation / resource. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;invalid_body&#x60;, &#x60;invalid_lookup_request&#x60; }.  |  -  |
**401** | Caller is not authenticated. |  -  |
**413** | Request body exceeded the 8 KiB ceiling enforced by the handler.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

