# plexsphere.CloudApi

All URIs are relative to *http://localhost*

Method | HTTP request | Description
------------- | ------------- | -------------
[**approve_credential_assignment**](CloudApi.md#approve_credential_assignment) | **POST** /v1/credential-assignments/{id}/approve | Approve a Credential Assignment.
[**create_cloud**](CloudApi.md#create_cloud) | **POST** /v1/clouds | Create a Cloud Inventory entry.
[**delete_cloud**](CloudApi.md#delete_cloud) | **DELETE** /v1/clouds/{id} | Delete a Cloud.
[**get_cloud**](CloudApi.md#get_cloud) | **GET** /v1/clouds/{id} | Fetch a Cloud by identifier.
[**get_cloud_credential**](CloudApi.md#get_cloud_credential) | **GET** /v1/cloud-credentials/{id} | Fetch a Cloud Credential&#39;s lifecycle metadata.
[**list_cloud_credentials**](CloudApi.md#list_cloud_credentials) | **GET** /v1/clouds/{id}/cloud-credentials | List Cloud Credentials owned by a Cloud.
[**list_clouds**](CloudApi.md#list_clouds) | **GET** /v1/clouds | List Cloud Inventory entries.
[**list_credential_assignments**](CloudApi.md#list_credential_assignments) | **GET** /v1/projects/{id}/credential-assignments | List the Credential Assignments owned by a Project.
[**patch_cloud**](CloudApi.md#patch_cloud) | **PATCH** /v1/clouds/{id} | Patch mutable fields on a Cloud.
[**reject_credential_assignment**](CloudApi.md#reject_credential_assignment) | **POST** /v1/credential-assignments/{id}/reject | Reject a Credential Assignment.
[**request_credential_assignment**](CloudApi.md#request_credential_assignment) | **POST** /v1/projects/{id}/credential-assignments | Request a Credential Assignment for a Project.
[**revoke_cloud_credential**](CloudApi.md#revoke_cloud_credential) | **POST** /v1/cloud-credentials/{id}/revoke | Revoke a Cloud Credential.
[**revoke_credential_assignment**](CloudApi.md#revoke_credential_assignment) | **POST** /v1/credential-assignments/{id}/revoke | Revoke a Credential Assignment.


# **approve_credential_assignment**
> CredentialAssignmentResponse approve_credential_assignment(id)

Approve a Credential Assignment.

Approves the Credential Assignment identified by `{id}`. The
handler reads the row to resolve the parent Project, runs the
`manage` ReBAC check on that Project, then delegates to the
Credential Assignment application service which moves the
assignment to the `approved` state, materialises the binding,
and appends a `CredentialAssignmentApproved` outbox event in a
single transaction.

Approval is only legal from the `requested` state — any other
source state returns `409 illegal_transition`. The caller may
not approve an assignment they themselves requested; that
self-approval is rejected with `403 self_approval_denied`.


### Example


```python
import plexsphere
from plexsphere.models.credential_assignment_response import CredentialAssignmentResponse
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
    api_instance = plexsphere.CloudApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Credential Assignment identifier (UUIDv7). Bound on `/v1/credential-assignments/{id}/approve`, `/v1/credential-assignments/{id}/reject`, and `/v1/credential-assignments/{id}/revoke` for the Credential Assignment decision surface. 

    try:
        # Approve a Credential Assignment.
        api_response = api_instance.approve_credential_assignment(id)
        print("The response of CloudApi->approve_credential_assignment:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling CloudApi->approve_credential_assignment: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Credential Assignment identifier (UUIDv7). Bound on &#x60;/v1/credential-assignments/{id}/approve&#x60;, &#x60;/v1/credential-assignments/{id}/reject&#x60;, and &#x60;/v1/credential-assignments/{id}/revoke&#x60; for the Credential Assignment decision surface.  | 

### Return type

[**CredentialAssignmentResponse**](CredentialAssignmentResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Credential Assignment approved. Body is the metadata projection with &#x60;state: approved&#x60; and &#x60;materialised: true&#x60;.  |  -  |
**400** | Malformed id. Body is a &#x60;Problem&#x60; with &#x60;code: invalid_credential_assignment_id&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to manage the parent Project, or the caller is the requester of this assignment and may not approve their own request (body is a &#x60;Problem&#x60; with &#x60;code: self_approval_denied&#x60;).  |  -  |
**404** | Credential Assignment not found. Body is a &#x60;Problem&#x60; with &#x60;code: credential_assignment_not_found&#x60;.  |  -  |
**409** | The assignment is not in a state from which approval is legal. Body is a &#x60;Problem&#x60; with &#x60;code: illegal_transition&#x60;.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **create_cloud**
> CloudResponse create_cloud(cloud_create_request)

Create a Cloud Inventory entry.

Creates a new Cloud aggregate — one upstream cloud-provider
account with its connection metadata. The aggregate enforces
every issuance invariant — non-empty `display_name`, kebab-case
`slug`, closed-enum `provider`, valid JSON `endpoint` and
`region_defaults`, non-empty `external_id`. The per-provider
validator runs BEFORE the aggregate so a malformed payload
never appends a `CloudCreated` outbox row; field-level
rejections surface as `400 invalid_cloud_endpoint` or
`400 invalid_cloud_region_defaults`. A duplicate slug surfaces
as `409 cloud_slug_conflict`; a duplicate
`(provider, external_id)` pair surfaces as
`409 cloud_external_id_conflict`.

On success the handler emits a `cloud.create` audit row and
appends a `CloudCreated` outbox event in the same transaction.


### Example


```python
import plexsphere
from plexsphere.models.cloud_create_request import CloudCreateRequest
from plexsphere.models.cloud_response import CloudResponse
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
    api_instance = plexsphere.CloudApi(api_client)
    cloud_create_request = {"display_name":"Acme AWS Production","slug":"acme-aws-prod","provider":"aws","endpoint":{"region":"us-east-1","role_arn":"arn:aws:iam::123456789012:role/PlexsphereProvisioner"},"region_defaults":{"vpc_cidr":"10.42.0.0/16"},"external_id":"123456789012"} # CloudCreateRequest | 

    try:
        # Create a Cloud Inventory entry.
        api_response = api_instance.create_cloud(cloud_create_request)
        print("The response of CloudApi->create_cloud:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling CloudApi->create_cloud: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **cloud_create_request** | [**CloudCreateRequest**](CloudCreateRequest.md)|  | 

### Return type

[**CloudResponse**](CloudResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**201** | Cloud created. |  -  |
**400** | Aggregate or validator rejected the body — empty &#x60;display_name&#x60;, malformed &#x60;slug&#x60;, malformed &#x60;endpoint&#x60; or &#x60;region_defaults&#x60;, unknown &#x60;provider&#x60;, or per-provider field-level validator rejection. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;invalid_cloud&#x60;, &#x60;unknown_provider&#x60;, &#x60;invalid_cloud_endpoint&#x60;, &#x60;invalid_cloud_region_defaults&#x60;, &#x60;invalid_body&#x60; }.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to create Clouds. Body is a &#x60;PermissionDenied&#x60; problem carrying the ReBAC denial &#x60;reason&#x60;, &#x60;relation_path&#x60;, and &#x60;correlation_id&#x60;.  |  -  |
**409** | Conflict — the proposed Cloud collides with a persisted row. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;cloud_slug_conflict&#x60;, &#x60;cloud_external_id_conflict&#x60; }.  |  -  |
**413** | Request body exceeded the 8 KiB Cloud Inventory ceiling enforced by the handler.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **delete_cloud**
> delete_cloud(id)

Delete a Cloud.

Deletes the Cloud identified by `{id}`. The empty-aggregate
guard runs inside the same transaction as the row delete; at
least one persisted `CloudCredential` forces
`409 cloud_not_empty` with the `CloudChildCounts` payload in
the Problem detail so the operator knows how many credentials
are still attached. A concurrent INSERT racing the guard is
caught by defense-in-depth — the foreign-key violation
surfaces as the same `409` so the caller never observes a
half-deleted Cloud.


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
    api_instance = plexsphere.CloudApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Cloud identifier (UUIDv7). Bound on `/v1/clouds/{id}` for the Cloud Inventory CRUD surface. 

    try:
        # Delete a Cloud.
        api_instance.delete_cloud(id)
    except Exception as e:
        print("Exception when calling CloudApi->delete_cloud: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Cloud identifier (UUIDv7). Bound on &#x60;/v1/clouds/{id}&#x60; for the Cloud Inventory CRUD surface.  | 

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
**204** | Cloud deleted. |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to delete the addressed Cloud. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Cloud not found. Body is a &#x60;Problem&#x60; with &#x60;code: cloud_not_found&#x60;.  |  -  |
**409** | Cloud still owns at least one child aggregate. Body is a &#x60;Problem&#x60; with &#x60;code: cloud_not_empty&#x60; and the optional &#x60;cloud_child_counts&#x60; extension carrying the structured &#x60;CloudChildCounts&#x60; so the operator knows how many credentials are still attached.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **get_cloud**
> CloudResponse get_cloud(id)

Fetch a Cloud by identifier.

Returns the Cloud identified by `{id}`. The handler runs the
`read` ReBAC check BEFORE the persistence read; an unauthorised
caller therefore receives `403` without the existence side-
channel a "load-then-check" flow would leak. A missing
aggregate surfaces as `404 cloud_not_found`.


### Example


```python
import plexsphere
from plexsphere.models.cloud_response import CloudResponse
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
    api_instance = plexsphere.CloudApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Cloud identifier (UUIDv7). Bound on `/v1/clouds/{id}` for the Cloud Inventory CRUD surface. 

    try:
        # Fetch a Cloud by identifier.
        api_response = api_instance.get_cloud(id)
        print("The response of CloudApi->get_cloud:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling CloudApi->get_cloud: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Cloud identifier (UUIDv7). Bound on &#x60;/v1/clouds/{id}&#x60; for the Cloud Inventory CRUD surface.  | 

### Return type

[**CloudResponse**](CloudResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Cloud found. |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to read the addressed Cloud. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Cloud not found. Body is a &#x60;Problem&#x60; with &#x60;code: cloud_not_found&#x60;.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **get_cloud_credential**
> CloudCredentialResponse get_cloud_credential(id)

Fetch a Cloud Credential's lifecycle metadata.

Returns the lifecycle metadata for the Cloud Credential
identified by `{id}`. The credential id does not encode its
owning Cloud, so the handler must read the row to learn which
Cloud to authorise against; the ReBAC `observe` check runs on
the resolved parent Cloud and a denial returns `403` via the
audit-first permission-denied path. A missing row surfaces as
`404 cloud_credential_not_found`.

The projection is metadata-only and NEVER exposes the KV mount,
KV path, KV version, or any secret material.


### Example


```python
import plexsphere
from plexsphere.models.cloud_credential_response import CloudCredentialResponse
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
    api_instance = plexsphere.CloudApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Cloud Credential identifier (UUIDv7). Bound on `/v1/cloud-credentials/{id}` and `/v1/cloud-credentials/{id}/revoke` for the operator-facing Cloud Credentials read + revoke surface. 

    try:
        # Fetch a Cloud Credential's lifecycle metadata.
        api_response = api_instance.get_cloud_credential(id)
        print("The response of CloudApi->get_cloud_credential:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling CloudApi->get_cloud_credential: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Cloud Credential identifier (UUIDv7). Bound on &#x60;/v1/cloud-credentials/{id}&#x60; and &#x60;/v1/cloud-credentials/{id}/revoke&#x60; for the operator-facing Cloud Credentials read + revoke surface.  | 

### Return type

[**CloudCredentialResponse**](CloudCredentialResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Cloud Credential found. |  -  |
**400** | Malformed Cloud Credential id. Body is a &#x60;Problem&#x60; with &#x60;code: invalid_cloud_credential_id&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to observe the parent Cloud. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Cloud Credential not found. Body is a &#x60;Problem&#x60; with &#x60;code: cloud_credential_not_found&#x60;.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **list_cloud_credentials**
> CloudCredentialList list_cloud_credentials(id, cursor=cursor, limit=limit)

List Cloud Credentials owned by a Cloud.

Returns a creation-ordered page of Cloud Credential lifecycle
metadata for the Cloud identified by `{id}`. The handler runs a
top-level `observe` ReBAC check on the parent Cloud BEFORE the
persistence read, then layers a per-row `observe` filter on top
so the response items are the subset of the persistence-level
page the caller is authorised to see.

The projection is metadata-only: it carries the credential
identity, version, lifecycle timestamps, and a derived status.
It NEVER exposes the KV mount, KV path, KV version, or any
secret material — the storage location is deliberately omitted
as a storage-internal detail.

The pagination cursor is HMAC-signed and bound to the
per-(caller, pepper) pseudonym, so a cursor minted by one
principal cannot be replayed by another — the cross-caller
replay surfaces as `403 cursor_binding_mismatch`. A tampered
envelope or unknown version byte stays on `400 invalid_cursor`.


### Example


```python
import plexsphere
from plexsphere.models.cloud_credential_list import CloudCredentialList
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
    api_instance = plexsphere.CloudApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Cloud identifier (UUIDv7). Bound on `/v1/clouds/{id}` for the Cloud Inventory CRUD surface. 
    cursor = 'cursor_example' # str | Opaque continuation token returned by a previous call's `next_cursor`. The encoding is HMAC-signed by the server so a tampered cursor surfaces as `400`.  (optional)
    limit = 50 # int | Maximum number of items to return in a single page. The handler clamps the value to [1, 200] before forwarding it to the read service.  (optional) (default to 50)

    try:
        # List Cloud Credentials owned by a Cloud.
        api_response = api_instance.list_cloud_credentials(id, cursor=cursor, limit=limit)
        print("The response of CloudApi->list_cloud_credentials:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling CloudApi->list_cloud_credentials: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Cloud identifier (UUIDv7). Bound on &#x60;/v1/clouds/{id}&#x60; for the Cloud Inventory CRUD surface.  | 
 **cursor** | **str**| Opaque continuation token returned by a previous call&#39;s &#x60;next_cursor&#x60;. The encoding is HMAC-signed by the server so a tampered cursor surfaces as &#x60;400&#x60;.  | [optional] 
 **limit** | **int**| Maximum number of items to return in a single page. The handler clamps the value to [1, 200] before forwarding it to the read service.  | [optional] [default to 50]

### Return type

[**CloudCredentialList**](CloudCredentialList.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Page of Cloud Credentials. |  -  |
**400** | Invalid query parameters — typically a tampered or malformed cursor, an out-of-range &#x60;limit&#x60;, or a malformed Cloud id.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to observe the parent Cloud (body is a &#x60;PermissionDenied&#x60; problem) OR the pagination cursor was minted by a different caller and the per-(caller, pepper) HMAC binding rejected the replay (body is a &#x60;Problem&#x60; with &#x60;code &#x3D; cursor_binding_mismatch&#x60;).  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **list_clouds**
> CloudList list_clouds(cursor=cursor, limit=limit)

List Cloud Inventory entries.

Returns a slug-ordered page of Cloud aggregates the caller is
authorised to see. Per-row visibility is layered on top of the
page: rows the caller cannot `read` are filtered out so the
response items are a subset of the persistence-level page.

The pagination cursor is HMAC-signed and bound to the
per-(caller, pepper) pseudonym, so a cursor minted by one
principal cannot be replayed by another — the cross-caller
replay surfaces as `403 cursor_binding_mismatch`. A tampered
envelope or unknown version byte stays on `400 invalid_cursor`.


### Example


```python
import plexsphere
from plexsphere.models.cloud_list import CloudList
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
    api_instance = plexsphere.CloudApi(api_client)
    cursor = 'cursor_example' # str | Opaque continuation token returned by a previous call's `next_cursor`. The encoding is HMAC-signed by the server so a tampered cursor surfaces as `400`.  (optional)
    limit = 50 # int | Maximum number of items to return in a single page. The handler clamps the value to [1, 200] before forwarding it to the service.  (optional) (default to 50)

    try:
        # List Cloud Inventory entries.
        api_response = api_instance.list_clouds(cursor=cursor, limit=limit)
        print("The response of CloudApi->list_clouds:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling CloudApi->list_clouds: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **cursor** | **str**| Opaque continuation token returned by a previous call&#39;s &#x60;next_cursor&#x60;. The encoding is HMAC-signed by the server so a tampered cursor surfaces as &#x60;400&#x60;.  | [optional] 
 **limit** | **int**| Maximum number of items to return in a single page. The handler clamps the value to [1, 200] before forwarding it to the service.  | [optional] [default to 50]

### Return type

[**CloudList**](CloudList.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Page of Clouds. |  -  |
**400** | Invalid query parameters — typically a tampered or malformed cursor or an out-of-range &#x60;limit&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to list Clouds (body is a &#x60;PermissionDenied&#x60; problem) OR the pagination cursor was minted by a different caller and the per-(caller, pepper) HMAC binding rejected the replay (body is a &#x60;Problem&#x60; with &#x60;code &#x3D; cursor_binding_mismatch&#x60;).  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **list_credential_assignments**
> CredentialAssignmentList list_credential_assignments(id, cursor=cursor, limit=limit)

List the Credential Assignments owned by a Project.

Returns a creation-ordered page of Credential Assignment
lifecycle metadata for the Project identified by `{id}`. The
handler runs a top-level `observe` ReBAC check on the parent
Project BEFORE the persistence read, then layers a per-row
`observe` filter on top so the response items are the subset
of the persistence-level page the caller is authorised to see.

The projection carries the assignment identity, the owning
Project, the bound Cloud Credential, the lifecycle state, a
derived `materialised` flag, and the lifecycle timestamps.

The pagination cursor is HMAC-signed and bound to the
per-(caller, pepper) pseudonym, so a cursor minted by one
principal cannot be replayed by another — the cross-caller
replay surfaces as `403 cursor_binding_mismatch`. A tampered
envelope or unknown version byte stays on `400 invalid_cursor`.


### Example


```python
import plexsphere
from plexsphere.models.credential_assignment_list import CredentialAssignmentList
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
    api_instance = plexsphere.CloudApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Project identifier (UUIDv7). Bound on `/v1/projects/{id}` for the tenancy CRUD surface and on `/v1/projects/{id}/credentials` for the operator-facing OpenBao Credential Broker inventory list. 
    cursor = 'cursor_example' # str | Opaque continuation token returned by a previous call's `next_cursor`. The encoding is HMAC-signed by the server so a tampered cursor surfaces as `400`.  (optional)
    limit = 50 # int | Maximum number of items to return in a single page. The handler clamps the value to [1, 200] before forwarding it to the read service.  (optional) (default to 50)

    try:
        # List the Credential Assignments owned by a Project.
        api_response = api_instance.list_credential_assignments(id, cursor=cursor, limit=limit)
        print("The response of CloudApi->list_credential_assignments:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling CloudApi->list_credential_assignments: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Project identifier (UUIDv7). Bound on &#x60;/v1/projects/{id}&#x60; for the tenancy CRUD surface and on &#x60;/v1/projects/{id}/credentials&#x60; for the operator-facing OpenBao Credential Broker inventory list.  | 
 **cursor** | **str**| Opaque continuation token returned by a previous call&#39;s &#x60;next_cursor&#x60;. The encoding is HMAC-signed by the server so a tampered cursor surfaces as &#x60;400&#x60;.  | [optional] 
 **limit** | **int**| Maximum number of items to return in a single page. The handler clamps the value to [1, 200] before forwarding it to the read service.  | [optional] [default to 50]

### Return type

[**CredentialAssignmentList**](CredentialAssignmentList.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Page of Credential Assignments. |  -  |
**400** | Invalid query parameters — typically a tampered or malformed cursor, an out-of-range &#x60;limit&#x60;, or a malformed Project id.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to observe the parent Project (body is a &#x60;PermissionDenied&#x60; problem) OR the pagination cursor was minted by a different caller and the per-(caller, pepper) HMAC binding rejected the replay (body is a &#x60;Problem&#x60; with &#x60;code &#x3D; cursor_binding_mismatch&#x60;).  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **patch_cloud**
> CloudResponse patch_cloud(id, cloud_patch_request)

Patch mutable fields on a Cloud.

Patches the Cloud identified by `{id}`. The body MUST set at
least one of `display_name`, `endpoint`, or `region_defaults`
— an empty body surfaces as `400 empty_patch`.

DECISION: BOTH `slug` and `provider` are intentionally NOT
patchable fields. The slug is the URL handle exported into
cached dashboard links and outbox projections; the provider is
the validator-routing key for the per-provider validator
family — changing either would silently break references or
invalidate every previously-stored endpoint blob. The handler
rejects any body that carries a `slug` key (even with the same
value) at decode time with `400 slug_immutable`, and any body
that carries a `provider` key with `400 provider_immutable`.
See the `cloud` tag description for the full rationale.

Mutating `endpoint` or `region_defaults` re-runs the per-
provider validator on the merged next-state so a stale
`region_defaults` cannot silently invalidate the merged record;
field-level validator rejections surface as
`400 invalid_cloud_endpoint` or
`400 invalid_cloud_region_defaults`.


### Example


```python
import plexsphere
from plexsphere.models.cloud_patch_request import CloudPatchRequest
from plexsphere.models.cloud_response import CloudResponse
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
    api_instance = plexsphere.CloudApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Cloud identifier (UUIDv7). Bound on `/v1/clouds/{id}` for the Cloud Inventory CRUD surface. 
    cloud_patch_request = {"display_name":"Acme AWS Production — renamed","region_defaults":{"vpc_cidr":"10.43.0.0/16"}} # CloudPatchRequest | 

    try:
        # Patch mutable fields on a Cloud.
        api_response = api_instance.patch_cloud(id, cloud_patch_request)
        print("The response of CloudApi->patch_cloud:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling CloudApi->patch_cloud: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Cloud identifier (UUIDv7). Bound on &#x60;/v1/clouds/{id}&#x60; for the Cloud Inventory CRUD surface.  | 
 **cloud_patch_request** | [**CloudPatchRequest**](CloudPatchRequest.md)|  | 

### Return type

[**CloudResponse**](CloudResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Cloud patched. |  -  |
**400** | Invalid body. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;invalid_cloud&#x60;, &#x60;invalid_cloud_endpoint&#x60;, &#x60;invalid_cloud_region_defaults&#x60;, &#x60;slug_immutable&#x60;, &#x60;provider_immutable&#x60;, &#x60;empty_patch&#x60;, &#x60;invalid_body&#x60; }.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to manage the addressed Cloud. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Cloud not found. Body is a &#x60;Problem&#x60; with &#x60;code: cloud_not_found&#x60;.  |  -  |
**413** | Request body exceeded the 8 KiB Cloud Inventory ceiling.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **reject_credential_assignment**
> CredentialAssignmentResponse reject_credential_assignment(id, credential_assignment_decision_request)

Reject a Credential Assignment.

Rejects the Credential Assignment identified by `{id}`. The
handler reads the row to resolve the parent Project, runs the
`manage` ReBAC check on that Project, then delegates to the
Credential Assignment application service which moves the
assignment to the `rejected` state and appends a
`CredentialAssignmentRejected` outbox event in a single
transaction. The `reason` from the body is recorded on the
event as an approver-supplied audit string.

Rejection is only legal from the `requested` state — any other
source state returns `409 illegal_transition`.


### Example


```python
import plexsphere
from plexsphere.models.credential_assignment_decision_request import CredentialAssignmentDecisionRequest
from plexsphere.models.credential_assignment_response import CredentialAssignmentResponse
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
    api_instance = plexsphere.CloudApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Credential Assignment identifier (UUIDv7). Bound on `/v1/credential-assignments/{id}/approve`, `/v1/credential-assignments/{id}/reject`, and `/v1/credential-assignments/{id}/revoke` for the Credential Assignment decision surface. 
    credential_assignment_decision_request = {"reason":"Cloud Credential is scheduled for rotation this week"} # CredentialAssignmentDecisionRequest | 

    try:
        # Reject a Credential Assignment.
        api_response = api_instance.reject_credential_assignment(id, credential_assignment_decision_request)
        print("The response of CloudApi->reject_credential_assignment:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling CloudApi->reject_credential_assignment: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Credential Assignment identifier (UUIDv7). Bound on &#x60;/v1/credential-assignments/{id}/approve&#x60;, &#x60;/v1/credential-assignments/{id}/reject&#x60;, and &#x60;/v1/credential-assignments/{id}/revoke&#x60; for the Credential Assignment decision surface.  | 
 **credential_assignment_decision_request** | [**CredentialAssignmentDecisionRequest**](CredentialAssignmentDecisionRequest.md)|  | 

### Return type

[**CredentialAssignmentResponse**](CredentialAssignmentResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Credential Assignment rejected. Body is the metadata projection with &#x60;state: rejected&#x60;.  |  -  |
**400** | Malformed id or body. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;invalid_credential_assignment_id&#x60;, &#x60;invalid_body&#x60;, &#x60;invalid_decision_reason&#x60; }.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to manage the parent Project. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Credential Assignment not found. Body is a &#x60;Problem&#x60; with &#x60;code: credential_assignment_not_found&#x60;.  |  -  |
**409** | The assignment is not in a state from which rejection is legal. Body is a &#x60;Problem&#x60; with &#x60;code: illegal_transition&#x60;.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **request_credential_assignment**
> CredentialAssignmentResponse request_credential_assignment(id, credential_assignment_request)

Request a Credential Assignment for a Project.

Opens a Credential Assignment request that binds the Cloud
Credential named in the body to the Project identified by
`{id}`. The handler runs a `manage` ReBAC check on the parent
Project BEFORE the persistence write, then delegates to the
Credential Assignment application service which records the
request in the `requested` state and appends a
`CredentialAssignmentRequested` outbox event in a single
transaction.

The newly opened assignment is not yet materialised — the
binding only becomes live once an approver moves it to the
`approved` state. A second open request for the same
(Project, Cloud Credential) pair while an earlier one is still
live is rejected with `409 duplicate_live_assignment`. A Cloud
Credential that is not in an assignable lifecycle state is
rejected with `422 credential_not_assignable`.


### Example


```python
import plexsphere
from plexsphere.models.credential_assignment_request import CredentialAssignmentRequest
from plexsphere.models.credential_assignment_response import CredentialAssignmentResponse
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
    api_instance = plexsphere.CloudApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Project identifier (UUIDv7). Bound on `/v1/projects/{id}` for the tenancy CRUD surface and on `/v1/projects/{id}/credentials` for the operator-facing OpenBao Credential Broker inventory list. 
    credential_assignment_request = {"cloud_credential_id":"0190a8b8-a0c0-7a0a-8a0a-a0a0a0a0a0d1"} # CredentialAssignmentRequest | 

    try:
        # Request a Credential Assignment for a Project.
        api_response = api_instance.request_credential_assignment(id, credential_assignment_request)
        print("The response of CloudApi->request_credential_assignment:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling CloudApi->request_credential_assignment: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Project identifier (UUIDv7). Bound on &#x60;/v1/projects/{id}&#x60; for the tenancy CRUD surface and on &#x60;/v1/projects/{id}/credentials&#x60; for the operator-facing OpenBao Credential Broker inventory list.  | 
 **credential_assignment_request** | [**CredentialAssignmentRequest**](CredentialAssignmentRequest.md)|  | 

### Return type

[**CredentialAssignmentResponse**](CredentialAssignmentResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**201** | Credential Assignment opened in the &#x60;requested&#x60; state. The &#x60;Location&#x60; header carries the canonical URL of the new assignment.  |  * Location - Absolute path of the issued Session for the single-Session read.  <br>  |
**400** | Malformed id or body. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;invalid_project_id&#x60;, &#x60;invalid_body&#x60;, &#x60;invalid_cloud_credential_id&#x60; }.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to manage the parent Project. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**409** | A live Credential Assignment already exists for the same (Project, Cloud Credential) pair. Body is a &#x60;Problem&#x60; with &#x60;code: duplicate_live_assignment&#x60;.  |  -  |
**422** | The named Cloud Credential is not in an assignable lifecycle state. Body is a &#x60;Problem&#x60; with &#x60;code: credential_not_assignable&#x60;.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **revoke_cloud_credential**
> CloudCredentialResponse revoke_cloud_credential(id, cloud_credential_revoke_request)

Revoke a Cloud Credential.

Revokes the Cloud Credential identified by `{id}`. The handler
reads the row to resolve the parent Cloud, runs the `manage`
ReBAC check on that Cloud, then delegates to the Cloud
Credentials Custodian which stamps `revoked_at`, soft-deletes
the underlying secret, and appends a `CloudCredentialRevoked`
outbox event in a single transaction.

Revocation is idempotent: revoking an already-revoked
credential returns `200` with the unchanged metadata rather
than an error. The response carries the metadata-only
projection showing the populated `revoked_at`.


### Example


```python
import plexsphere
from plexsphere.models.cloud_credential_response import CloudCredentialResponse
from plexsphere.models.cloud_credential_revoke_request import CloudCredentialRevokeRequest
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
    api_instance = plexsphere.CloudApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Cloud Credential identifier (UUIDv7). Bound on `/v1/cloud-credentials/{id}` and `/v1/cloud-credentials/{id}/revoke` for the operator-facing Cloud Credentials read + revoke surface. 
    cloud_credential_revoke_request = {"reason":"rotated out of band by the platform on-call"} # CloudCredentialRevokeRequest | 

    try:
        # Revoke a Cloud Credential.
        api_response = api_instance.revoke_cloud_credential(id, cloud_credential_revoke_request)
        print("The response of CloudApi->revoke_cloud_credential:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling CloudApi->revoke_cloud_credential: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Cloud Credential identifier (UUIDv7). Bound on &#x60;/v1/cloud-credentials/{id}&#x60; and &#x60;/v1/cloud-credentials/{id}/revoke&#x60; for the operator-facing Cloud Credentials read + revoke surface.  | 
 **cloud_credential_revoke_request** | [**CloudCredentialRevokeRequest**](CloudCredentialRevokeRequest.md)|  | 

### Return type

[**CloudCredentialResponse**](CloudCredentialResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Cloud Credential revoked (or already revoked — the call is idempotent). Body is the metadata-only projection with &#x60;revoked_at&#x60; populated.  |  -  |
**400** | Malformed id or body. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;invalid_cloud_credential_id&#x60;, &#x60;invalid_body&#x60;, &#x60;invalid_revoke_reason&#x60; }.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to manage the parent Cloud. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Cloud Credential not found. Body is a &#x60;Problem&#x60; with &#x60;code: cloud_credential_not_found&#x60;.  |  -  |
**413** | Request body exceeded the 8 KiB Cloud Credentials ceiling.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **revoke_credential_assignment**
> CredentialAssignmentResponse revoke_credential_assignment(id, credential_assignment_decision_request)

Revoke a Credential Assignment.

Revokes the Credential Assignment identified by `{id}`. The
handler reads the row to resolve the parent Project, runs the
`manage` ReBAC check on that Project, then delegates to the
Credential Assignment application service which moves the
assignment to the `revoked` state, tears down the materialised
binding, and appends a `CredentialAssignmentRevoked` outbox
event in a single transaction. The `reason` from the body is
recorded on the event as an operator-supplied audit string.

Revocation is only legal from the `approved` state — any other
source state returns `409 illegal_transition`.


### Example


```python
import plexsphere
from plexsphere.models.credential_assignment_decision_request import CredentialAssignmentDecisionRequest
from plexsphere.models.credential_assignment_response import CredentialAssignmentResponse
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
    api_instance = plexsphere.CloudApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Credential Assignment identifier (UUIDv7). Bound on `/v1/credential-assignments/{id}/approve`, `/v1/credential-assignments/{id}/reject`, and `/v1/credential-assignments/{id}/revoke` for the Credential Assignment decision surface. 
    credential_assignment_decision_request = {"reason":"Project decommissioned by the platform on-call"} # CredentialAssignmentDecisionRequest | 

    try:
        # Revoke a Credential Assignment.
        api_response = api_instance.revoke_credential_assignment(id, credential_assignment_decision_request)
        print("The response of CloudApi->revoke_credential_assignment:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling CloudApi->revoke_credential_assignment: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Credential Assignment identifier (UUIDv7). Bound on &#x60;/v1/credential-assignments/{id}/approve&#x60;, &#x60;/v1/credential-assignments/{id}/reject&#x60;, and &#x60;/v1/credential-assignments/{id}/revoke&#x60; for the Credential Assignment decision surface.  | 
 **credential_assignment_decision_request** | [**CredentialAssignmentDecisionRequest**](CredentialAssignmentDecisionRequest.md)|  | 

### Return type

[**CredentialAssignmentResponse**](CredentialAssignmentResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Credential Assignment revoked. Body is the metadata projection with &#x60;state: revoked&#x60; and &#x60;materialised: false&#x60;.  |  -  |
**400** | Malformed id or body. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;invalid_credential_assignment_id&#x60;, &#x60;invalid_body&#x60;, &#x60;invalid_decision_reason&#x60; }.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to manage the parent Project. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Credential Assignment not found. Body is a &#x60;Problem&#x60; with &#x60;code: credential_assignment_not_found&#x60;.  |  -  |
**409** | The assignment is not in a state from which revocation is legal. Body is a &#x60;Problem&#x60; with &#x60;code: illegal_transition&#x60;.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

