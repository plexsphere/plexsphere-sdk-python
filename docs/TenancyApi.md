# plexsphere.TenancyApi

All URIs are relative to *http://localhost*

Method | HTTP request | Description
------------- | ------------- | -------------
[**create_domain**](TenancyApi.md#create_domain) | **POST** /v1/domains | Create a tenancy Domain.
[**create_invitation**](TenancyApi.md#create_invitation) | **POST** /v1/domains/{id}/invitations | Stage a pending Invitation on a Domain.
[**create_project**](TenancyApi.md#create_project) | **POST** /v1/projects | Create a tenancy Project.
[**create_service_identity**](TenancyApi.md#create_service_identity) | **POST** /v1/domains/{id}/service-identities | Create a service identity on a Domain.
[**delete_domain**](TenancyApi.md#delete_domain) | **DELETE** /v1/domains/{id} | Delete a Domain.
[**delete_project**](TenancyApi.md#delete_project) | **DELETE** /v1/projects/{id} | Delete a Project.
[**get_domain**](TenancyApi.md#get_domain) | **GET** /v1/domains/{id} | Fetch a Domain by identifier.
[**get_identity**](TenancyApi.md#get_identity) | **GET** /v1/domains/{id}/identities/{principalId} | Fetch a single Domain principal by identifier.
[**get_invitation**](TenancyApi.md#get_invitation) | **GET** /v1/domains/{id}/invitations/{invitationId} | Fetch a single Invitation by identifier.
[**get_project**](TenancyApi.md#get_project) | **GET** /v1/projects/{id} | Fetch a Project by identifier.
[**list_domains**](TenancyApi.md#list_domains) | **GET** /v1/domains | List tenancy Domains.
[**list_identities**](TenancyApi.md#list_identities) | **GET** /v1/domains/{id}/identities | List principals (users + service identities) on a Domain.
[**list_invitations**](TenancyApi.md#list_invitations) | **GET** /v1/domains/{id}/invitations | List Invitations on a Domain.
[**list_projects**](TenancyApi.md#list_projects) | **GET** /v1/projects | List tenancy Projects.
[**patch_domain**](TenancyApi.md#patch_domain) | **PATCH** /v1/domains/{id} | Patch mutable fields on a Domain.
[**patch_project**](TenancyApi.md#patch_project) | **PATCH** /v1/projects/{id} | Patch mutable fields on a Project.
[**revoke_invitation**](TenancyApi.md#revoke_invitation) | **DELETE** /v1/domains/{id}/invitations/{invitationId} | Revoke a pending Invitation.


# **create_domain**
> DomainResponse create_domain(domain_create_request)

Create a tenancy Domain.

Creates a new top-level tenancy Domain. The
aggregate enforces every issuance invariant — non-empty Name,
kebab-case Slug, canonical RFC 4632 mesh_cidr, and a complete
ReachabilityPolicy (a fully-zero policy is replaced with the
platform default; a partial policy is rejected). Cross-Domain
mesh_cidr non-overlap is policed by the SQL GIST exclusion on
`plexsphere.domains.mesh_cidr` and surfaces as
`409 mesh_cidr_overlap`; a duplicate slug surfaces as
`409 domain_slug_conflict`.

On success the handler emits a `domain.create` audit row and
appends a `DomainCreated` outbox event in the same transaction
.


### Example


```python
import plexsphere
from plexsphere.models.domain_create_request import DomainCreateRequest
from plexsphere.models.domain_response import DomainResponse
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
    api_instance = plexsphere.TenancyApi(api_client)
    domain_create_request = {"name":"Acme Production","slug":"acme-prod","description":"Acme Corp production tenancy boundary.","mesh_cidr":"10.42.0.0/16","reachability":{"heartbeat_interval":"30s","stale_after":"90s","unreachable_after":"300s"}} # DomainCreateRequest | 

    try:
        # Create a tenancy Domain.
        api_response = api_instance.create_domain(domain_create_request)
        print("The response of TenancyApi->create_domain:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling TenancyApi->create_domain: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **domain_create_request** | [**DomainCreateRequest**](DomainCreateRequest.md)|  | 

### Return type

[**DomainResponse**](DomainResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**201** | Domain created. |  -  |
**400** | Aggregate invariant rejected the body — empty Name, malformed Slug, malformed &#x60;mesh_cidr&#x60;, partial &#x60;reachability&#x60; policy, or otherwise. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;invalid_domain&#x60;, &#x60;invalid_reachability_policy&#x60; }.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to create Domains. Body is a &#x60;PermissionDenied&#x60; problem carrying the ReBAC denial &#x60;reason&#x60;, &#x60;relation_path&#x60;, and &#x60;correlation_id&#x60;.  |  -  |
**409** | Conflict — the proposed Domain collides with a persisted Domain. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;domain_slug_conflict&#x60;, &#x60;mesh_cidr_overlap&#x60; }.  |  -  |
**413** | Request body exceeded the 8 KiB tenancy ceiling enforced by the handler.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **create_invitation**
> InvitationResponse create_invitation(id, invitation_create_request)

Stage a pending Invitation on a Domain.

Creates a pending Invitation under the Domain identified by
`{id}`. The handler authorises the call
against the parent Domain's `manage` ReBAC relation BEFORE
invoking the service so an unauthorised caller never produces
an `InvitationCreated` outbox row.

The aggregate enforces every issuance invariant — non-empty
`external_subject`, `ttl_seconds` in [60, 604800], at most 32
`initial_tuples`, and every `initial_tuples[].object` in the
closed prefix set
`{domain:<this-id>, project:<uuid>, group:<uuid>}`. A
duplicate `(domain_id, external_subject)` for which a pending
row already exists surfaces as
`409 invitation_already_pending` carrying the existing
invitation's id in the Problem detail so the operator can
choose to revoke and re-issue. The plaintext `external_subject`
is intentionally NOT echoed in the response — only the
per-Domain pseudonym is — so a server-side log of the response
body never carries raw IdP-side PII.

On success the handler emits an `invitation.create` audit row
and the service appends an `InvitationCreated` outbox event in
the same transaction.


### Example


```python
import plexsphere
from plexsphere.models.invitation_create_request import InvitationCreateRequest
from plexsphere.models.invitation_response import InvitationResponse
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
    api_instance = plexsphere.TenancyApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Domain identifier (UUIDv7). Bound on `/v1/domains/{id}` for the tenancy CRUD surface. 
    invitation_create_request = {"external_subject":"ada@example.com","ttl_seconds":86400,"initial_tuples":[{"relation":"member","object":"project:0190a8b8-a0c0-7a0a-8a0a-a0a0a0a0a0aa"}]} # InvitationCreateRequest | 

    try:
        # Stage a pending Invitation on a Domain.
        api_response = api_instance.create_invitation(id, invitation_create_request)
        print("The response of TenancyApi->create_invitation:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling TenancyApi->create_invitation: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Domain identifier (UUIDv7). Bound on &#x60;/v1/domains/{id}&#x60; for the tenancy CRUD surface.  | 
 **invitation_create_request** | [**InvitationCreateRequest**](InvitationCreateRequest.md)|  | 

### Return type

[**InvitationResponse**](InvitationResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**201** | Invitation staged. |  -  |
**400** | Aggregate or handler invariant rejected the body. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;invalid_body&#x60;, &#x60;invalid_ttl&#x60; }.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the &#x60;manage&#x60; ReBAC relation on the addressed Domain. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Domain not found. Surfaced only after the authorisation gate has passed.  |  -  |
**409** | A pending invitation already exists for the &#x60;(domain_id, external_subject)&#x60; pair. Body is a &#x60;Problem&#x60; with &#x60;code: invitation_already_pending&#x60;; the existing invitation&#39;s id is carried in the Problem detail so the operator can revoke-and-reissue without listing the table .  |  -  |
**413** | Request body exceeded the 8 KiB tenancy ceiling enforced by the handler.  |  -  |
**422** | Body parsed but a structural invariant rejected it. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;too_many_initial_tuples&#x60;, &#x60;invitation_object_out_of_scope&#x60;, &#x60;invalid_caveat_context&#x60; }.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **create_project**
> ProjectResponse create_project(project_create_request)

Create a tenancy Project.

Creates a new Project under the parent Domain identified by
`domain_id`. The aggregate enforces every
issuance invariant — non-zero parent Domain id, non-empty Name,
kebab-case Slug, optional canonical RFC 4632 sub-range
reservation. Cross-Project sub-range non-overlap inside the
parent Domain is policed by the SQL GIST exclusion on
`plexsphere.project_mesh_ip_reservations` and surfaces as
`409 sub_range_overlap`; a duplicate (domain_id, slug) surfaces
as `409 project_slug_conflict`.

On success the handler emits a `project.create` audit row and
appends a `ProjectCreated` outbox event in the same transaction
.


### Example


```python
import plexsphere
from plexsphere.models.project_create_request import ProjectCreateRequest
from plexsphere.models.project_response import ProjectResponse
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
    api_instance = plexsphere.TenancyApi(api_client)
    project_create_request = {"domain_id":"0190a8b8-a0c0-7a0a-8a0a-a0a0a0a0a0a1","name":"Acme Web","slug":"acme-web","description":"Web tier of Acme production.","sub_range_cidr":"10.42.4.0/22"} # ProjectCreateRequest | 

    try:
        # Create a tenancy Project.
        api_response = api_instance.create_project(project_create_request)
        print("The response of TenancyApi->create_project:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling TenancyApi->create_project: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **project_create_request** | [**ProjectCreateRequest**](ProjectCreateRequest.md)|  | 

### Return type

[**ProjectResponse**](ProjectResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**201** | Project created. |  -  |
**400** | Aggregate invariant rejected the body — empty Name, malformed Slug, malformed &#x60;sub_range_cidr&#x60;, etc. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;invalid_project&#x60;, &#x60;invalid_body&#x60; }.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to create Projects under the parent Domain. Body is a &#x60;PermissionDenied&#x60; problem carrying the ReBAC denial &#x60;reason&#x60;, &#x60;relation_path&#x60;, and &#x60;correlation_id&#x60;.  |  -  |
**409** | Conflict — the proposed Project collides with a persisted Project in the same Domain. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;project_slug_conflict&#x60;, &#x60;sub_range_overlap&#x60;, &#x60;parent_domain_missing&#x60; }.  |  -  |
**413** | Request body exceeded the 8 KiB tenancy ceiling enforced by the handler.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **create_service_identity**
> IdentitySummary create_service_identity(id, service_identity_create_request)

Create a service identity on a Domain.

Provisions a new machine principal — an identity of kind
`service-identity` — on the addressed Domain. The handler runs the
`manage` ReBAC check on `domain:<id>` BEFORE the persistence write,
so an unauthorised caller never produces a
`ServiceIdentityProvisioned` outbox row. On success the service
identity and its registration event are written in one transaction;
the authz projection turns that event into the
`serviceaccount:<id>#parent@domain:<id>` ReBAC edge that makes the
new row visible on `GET /v1/domains/{id}/identities`.

Human users are NOT created through this endpoint — they arrive by
accepting an Invitation (`POST /v1/domains/{id}/invitations`). This
collection provisions service identities only.

The `201` body is the same `IdentitySummary` projection the listing
surface returns, with `kind: service-identity` and the per-Domain
`external_subject_pseudonym` populated, so the client renders the
new row without a follow-up read. The plaintext `subject` is never
echoed back.


### Example


```python
import plexsphere
from plexsphere.models.identity_summary import IdentitySummary
from plexsphere.models.service_identity_create_request import ServiceIdentityCreateRequest
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
    api_instance = plexsphere.TenancyApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Domain identifier (UUIDv7). Bound on `/v1/domains/{id}` for the tenancy CRUD surface. 
    service_identity_create_request = plexsphere.ServiceIdentityCreateRequest() # ServiceIdentityCreateRequest | 

    try:
        # Create a service identity on a Domain.
        api_response = api_instance.create_service_identity(id, service_identity_create_request)
        print("The response of TenancyApi->create_service_identity:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling TenancyApi->create_service_identity: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Domain identifier (UUIDv7). Bound on &#x60;/v1/domains/{id}&#x60; for the tenancy CRUD surface.  | 
 **service_identity_create_request** | [**ServiceIdentityCreateRequest**](ServiceIdentityCreateRequest.md)|  | 

### Return type

[**IdentitySummary**](IdentitySummary.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**201** | The service identity was created. |  * Location - Absolute path of the issued Session for the single-Session read.  <br>  |
**400** | Invalid request body — a missing or whitespace-only &#x60;display_name&#x60;, &#x60;subject&#x60;, or &#x60;audience&#x60;, or a &#x60;federation_kind&#x60; outside the closed set {oidc_cc, spiffe_svid, api_token} (&#x60;code: invalid_service_identity&#x60;).  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the &#x60;manage&#x60; ReBAC relation on the addressed Domain (body is a &#x60;PermissionDenied&#x60; problem).  |  -  |
**409** | A service identity with the same &#x60;subject&#x60; already exists in this Domain (&#x60;code: service_identity_already_exists&#x60;). The (Domain, subject) pair is unique.  |  -  |
**413** | Request body exceeded the size cap. |  -  |
**503** | The authorization backend was unreachable, so the &#x60;manage&#x60; gate could not be evaluated. The condition is transient; retry with backoff.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **delete_domain**
> delete_domain(id)

Delete a Domain.

Deletes the Domain identified by `{id}`.
The empty-aggregate guard runs inside the same transaction as
the row delete; at least one persisted Project, Group,
Identity, IdP binding, or Node forces `409 domain_not_empty`
with the `DomainChildCounts` payload in the Problem detail so
the operator knows which sub-aggregate to drain first. A
concurrent INSERT racing the guard is caught by
defense-in-depth — the foreign-key violation surfaces as the
same `409` so the caller never observes a half-deleted Domain.


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
    api_instance = plexsphere.TenancyApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Domain identifier (UUIDv7). Bound on `/v1/domains/{id}` for the tenancy CRUD surface. 

    try:
        # Delete a Domain.
        api_instance.delete_domain(id)
    except Exception as e:
        print("Exception when calling TenancyApi->delete_domain: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Domain identifier (UUIDv7). Bound on &#x60;/v1/domains/{id}&#x60; for the tenancy CRUD surface.  | 

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
**204** | Domain deleted. |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to delete the addressed Domain. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Domain not found. Body is a &#x60;Problem&#x60; with &#x60;code: domain_not_found&#x60;.  |  -  |
**409** | Domain still owns at least one child aggregate. Body is a &#x60;Problem&#x60; with &#x60;code: domain_not_empty&#x60; and the optional &#x60;child_counts&#x60; extension carrying the structured &#x60;DomainChildCounts&#x60; so the operator knows which sub- aggregate is still attached.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **delete_project**
> delete_project(id)

Delete a Project.

Deletes the Project identified by `{id}`.
The empty-aggregate guard runs inside the same transaction as
the row delete; at least one persisted Resource, Node, or
relation tuple forces `409 project_not_empty` with the
`ProjectChildCounts` payload in the Problem detail so the
operator knows which sub-aggregate to drain first. A
concurrent INSERT racing the guard is caught by
defense-in-depth — the foreign-key violation surfaces as the
same `409` so the caller never observes a half-deleted
Project.


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
    api_instance = plexsphere.TenancyApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Project identifier (UUIDv7). Bound on `/v1/projects/{id}` for the tenancy CRUD surface and on `/v1/projects/{id}/credentials` for the operator-facing OpenBao Credential Broker inventory list. 

    try:
        # Delete a Project.
        api_instance.delete_project(id)
    except Exception as e:
        print("Exception when calling TenancyApi->delete_project: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Project identifier (UUIDv7). Bound on &#x60;/v1/projects/{id}&#x60; for the tenancy CRUD surface and on &#x60;/v1/projects/{id}/credentials&#x60; for the operator-facing OpenBao Credential Broker inventory list.  | 

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
**204** | Project deleted. |  -  |
**400** | Path &#x60;{id}&#x60; was not a non-zero UUID. Body is a &#x60;Problem&#x60; with &#x60;code: invalid_project_id&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to delete the addressed Project. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Project not found. Body is a &#x60;Problem&#x60; with &#x60;code: project_not_found&#x60;.  |  -  |
**409** | Project still owns at least one child aggregate. Body is a &#x60;Problem&#x60; with &#x60;code: project_not_empty&#x60; and the optional &#x60;project_child_counts&#x60; extension carrying the structured &#x60;ProjectChildCounts&#x60; so the operator knows which sub-aggregate is still attached.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **get_domain**
> DomainResponse get_domain(id)

Fetch a Domain by identifier.

Returns the Domain identified by `{id}`. The
handler runs the `read` ReBAC check BEFORE the persistence
read; an unauthorised caller therefore receives `403` without
the existence side-channel a "load-then-check" flow would
leak. A missing aggregate surfaces as `404 domain_not_found`.


### Example


```python
import plexsphere
from plexsphere.models.domain_response import DomainResponse
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
    api_instance = plexsphere.TenancyApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Domain identifier (UUIDv7). Bound on `/v1/domains/{id}` for the tenancy CRUD surface. 

    try:
        # Fetch a Domain by identifier.
        api_response = api_instance.get_domain(id)
        print("The response of TenancyApi->get_domain:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling TenancyApi->get_domain: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Domain identifier (UUIDv7). Bound on &#x60;/v1/domains/{id}&#x60; for the tenancy CRUD surface.  | 

### Return type

[**DomainResponse**](DomainResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Domain found. |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to read the addressed Domain. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Domain not found. Body is a &#x60;Problem&#x60; with &#x60;code: domain_not_found&#x60;.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **get_identity**
> IdentityDetail get_identity(id, principal_id)

Fetch a single Domain principal by identifier.

Returns the principal identified by `{principalId}` inside the
Domain identified by `{id}`. The handler
runs the `read` ReBAC check on `domain:<id>` BEFORE the
persistence read; an unauthorised caller therefore receives
`403` without the existence side-channel a "load-then-check"
flow would leak. A missing principal — OR a principal that
exists in a different Domain — surfaces as `404
identity_not_found` so the endpoint cannot be used as a
cross-Domain enumeration oracle.

The plaintext `external_subject` and `email` fields are
populated ONLY when the caller carries the `auditor` relation
on the addressed Domain (`domain:<id>#auditor`). A
`read`-only caller receives the same shape with the plaintext
fields elided so a client cannot escalate by reading the wire
bytes.

Every served byte is paired with an `identity.read` audit row
.


### Example


```python
import plexsphere
from plexsphere.models.identity_detail import IdentityDetail
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
    api_instance = plexsphere.TenancyApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Domain identifier (UUIDv7). Bound on `/v1/domains/{id}` for the tenancy CRUD surface. 
    principal_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Principal identifier (UUIDv7). Bound on `/v1/domains/{id}/identities/{principalId}` for the per-Domain identity-read surface. 

    try:
        # Fetch a single Domain principal by identifier.
        api_response = api_instance.get_identity(id, principal_id)
        print("The response of TenancyApi->get_identity:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling TenancyApi->get_identity: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Domain identifier (UUIDv7). Bound on &#x60;/v1/domains/{id}&#x60; for the tenancy CRUD surface.  | 
 **principal_id** | **UUID**| Principal identifier (UUIDv7). Bound on &#x60;/v1/domains/{id}/identities/{principalId}&#x60; for the per-Domain identity-read surface.  | 

### Return type

[**IdentityDetail**](IdentityDetail.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Principal found. |  -  |
**400** | Path &#x60;{principalId}&#x60; was not a non-zero UUID. Body is a &#x60;Problem&#x60; with &#x60;code: invalid_principal_id&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the &#x60;read&#x60; ReBAC relation on the addressed Domain. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Principal does not exist or belongs to a different Domain. Body is a &#x60;Problem&#x60; with &#x60;code: identity_not_found&#x60; .  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **get_invitation**
> InvitationResponse get_invitation(id, invitation_id)

Fetch a single Invitation by identifier.

Returns the Invitation identified by `{invitationId}` inside
the Domain identified by `{id}`. The
handler runs the `read` ReBAC check on `domain:<id>` BEFORE
the persistence read; an unauthorised caller receives `403`
without the existence side-channel a "load-then-check" flow
would leak. A missing aggregate — OR an aggregate that exists
in a different Domain — surfaces as `404 invitation_not_found`
so the endpoint cannot be used as a cross-Domain enumeration
oracle.

Every served byte is paired with an `invitation.read` audit
row.


### Example


```python
import plexsphere
from plexsphere.models.invitation_response import InvitationResponse
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
    api_instance = plexsphere.TenancyApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Domain identifier (UUIDv7). Bound on `/v1/domains/{id}` for the tenancy CRUD surface. 
    invitation_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Invitation identifier (UUIDv7). Bound on `/v1/domains/{id}/invitations/{invitationId}` for the per- Domain invitation read / revoke surface. 

    try:
        # Fetch a single Invitation by identifier.
        api_response = api_instance.get_invitation(id, invitation_id)
        print("The response of TenancyApi->get_invitation:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling TenancyApi->get_invitation: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Domain identifier (UUIDv7). Bound on &#x60;/v1/domains/{id}&#x60; for the tenancy CRUD surface.  | 
 **invitation_id** | **UUID**| Invitation identifier (UUIDv7). Bound on &#x60;/v1/domains/{id}/invitations/{invitationId}&#x60; for the per- Domain invitation read / revoke surface.  | 

### Return type

[**InvitationResponse**](InvitationResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Invitation found. |  -  |
**400** | Path &#x60;{invitationId}&#x60; was not a non-zero UUID. Body is a &#x60;Problem&#x60; with &#x60;code: invalid_invitation_id&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the &#x60;read&#x60; ReBAC relation on the addressed Domain. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Invitation does not exist or belongs to a different Domain. Body is a &#x60;Problem&#x60; with &#x60;code: invitation_not_found&#x60;.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **get_project**
> ProjectResponse get_project(id)

Fetch a Project by identifier.

Returns the Project identified by `{id}`.
The handler runs the `read` ReBAC check BEFORE the persistence
read; an unauthorised caller therefore receives `403` without
the existence side-channel a "load-then-check" flow would
leak. A missing aggregate surfaces as `404 project_not_found`.


### Example


```python
import plexsphere
from plexsphere.models.project_response import ProjectResponse
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
    api_instance = plexsphere.TenancyApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Project identifier (UUIDv7). Bound on `/v1/projects/{id}` for the tenancy CRUD surface and on `/v1/projects/{id}/credentials` for the operator-facing OpenBao Credential Broker inventory list. 

    try:
        # Fetch a Project by identifier.
        api_response = api_instance.get_project(id)
        print("The response of TenancyApi->get_project:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling TenancyApi->get_project: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Project identifier (UUIDv7). Bound on &#x60;/v1/projects/{id}&#x60; for the tenancy CRUD surface and on &#x60;/v1/projects/{id}/credentials&#x60; for the operator-facing OpenBao Credential Broker inventory list.  | 

### Return type

[**ProjectResponse**](ProjectResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Project found. |  -  |
**400** | Path &#x60;{id}&#x60; was not a non-zero UUID. Body is a &#x60;Problem&#x60; with &#x60;code: invalid_project_id&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to read the addressed Project. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Project not found. Body is a &#x60;Problem&#x60; with &#x60;code: project_not_found&#x60;.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **list_domains**
> DomainList list_domains(cursor=cursor, limit=limit)

List tenancy Domains.

Returns a slug-ordered page of tenancy Domains the caller is
authorised to see. Per-row
visibility is layered on top of the page: rows the caller
cannot `read` are filtered out so the response items are a
subset of the persistence-level page.

The pagination cursor is HMAC-signed and bound to the
per-(caller, pepper) pseudonym, so a cursor minted by one
principal cannot be replayed by another — the cross-caller
replay surfaces as `403 cursor_binding_mismatch`. A tampered
envelope or unknown version byte stays on `400 invalid_cursor`.


### Example


```python
import plexsphere
from plexsphere.models.domain_list import DomainList
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
    api_instance = plexsphere.TenancyApi(api_client)
    cursor = 'cursor_example' # str | Opaque continuation token returned by a previous call's `next_cursor`. The encoding is HMAC-signed by the server so a tampered cursor surfaces as `400`.  (optional)
    limit = 50 # int | Maximum number of items to return in a single page. The handler clamps the value to [1, 200] before forwarding it to the service.  (optional) (default to 50)

    try:
        # List tenancy Domains.
        api_response = api_instance.list_domains(cursor=cursor, limit=limit)
        print("The response of TenancyApi->list_domains:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling TenancyApi->list_domains: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **cursor** | **str**| Opaque continuation token returned by a previous call&#39;s &#x60;next_cursor&#x60;. The encoding is HMAC-signed by the server so a tampered cursor surfaces as &#x60;400&#x60;.  | [optional] 
 **limit** | **int**| Maximum number of items to return in a single page. The handler clamps the value to [1, 200] before forwarding it to the service.  | [optional] [default to 50]

### Return type

[**DomainList**](DomainList.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Page of Domains. |  -  |
**400** | Invalid query parameters — typically a tampered or malformed cursor.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to list Domains (body is a &#x60;PermissionDenied&#x60; problem) OR the pagination cursor was minted by a different caller and the per-(caller, pepper) HMAC binding rejected the replay (body is a &#x60;Problem&#x60; with &#x60;code &#x3D; cursor_binding_mismatch&#x60;).  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **list_identities**
> IdentityList list_identities(id, cursor=cursor, limit=limit, kind=kind)

List principals (users + service identities) on a Domain.

Returns a cursor-paginated page of principals attached to the
addressed Domain. The handler runs the
`read` ReBAC check on `domain:<id>` BEFORE the persistence
read; an unauthorised caller therefore receives `403` without
the existence side-channel a "load-then-check" flow would
leak. The optional `kind` query parameter narrows the page to
a single principal kind (`user` or `service-identity`); omit
to page across both kinds in the same Domain.

The summary projection NEVER carries plaintext
`external_subject` or `email`. Auditor-only fields are reachable
only via `GET /v1/domains/{id}/identities/{principalId}`
.

The pagination cursor is HMAC-signed and bound to the
per-(caller, pepper) pseudonym, so a cursor minted by one
principal cannot be replayed by another — the cross-caller
replay surfaces as `403 cursor_binding_mismatch`. A tampered
envelope or unknown version byte stays on `400 invalid_cursor`.

On every served page the handler emits one
`identity.list` audit row.


### Example


```python
import plexsphere
from plexsphere.models.identity_kind import IdentityKind
from plexsphere.models.identity_list import IdentityList
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
    api_instance = plexsphere.TenancyApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Domain identifier (UUIDv7). Bound on `/v1/domains/{id}` for the tenancy CRUD surface. 
    cursor = 'cursor_example' # str | Opaque continuation token returned by a previous call's `next_cursor`. The encoding is HMAC-signed by the server so a tampered cursor surfaces as `400 invalid_cursor` .  (optional)
    limit = 50 # int | Maximum number of items to return in a single page. The handler clamps the value to [1, 200] before forwarding it to the service.  (optional) (default to 50)
    kind = plexsphere.IdentityKind() # IdentityKind | Optional principal-kind filter. Values outside the closed set surface as `400 invalid_kind`.  (optional)

    try:
        # List principals (users + service identities) on a Domain.
        api_response = api_instance.list_identities(id, cursor=cursor, limit=limit, kind=kind)
        print("The response of TenancyApi->list_identities:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling TenancyApi->list_identities: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Domain identifier (UUIDv7). Bound on &#x60;/v1/domains/{id}&#x60; for the tenancy CRUD surface.  | 
 **cursor** | **str**| Opaque continuation token returned by a previous call&#39;s &#x60;next_cursor&#x60;. The encoding is HMAC-signed by the server so a tampered cursor surfaces as &#x60;400 invalid_cursor&#x60; .  | [optional] 
 **limit** | **int**| Maximum number of items to return in a single page. The handler clamps the value to [1, 200] before forwarding it to the service.  | [optional] [default to 50]
 **kind** | [**IdentityKind**](.md)| Optional principal-kind filter. Values outside the closed set surface as &#x60;400 invalid_kind&#x60;.  | [optional] 

### Return type

[**IdentityList**](IdentityList.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Page of identity summaries. |  -  |
**400** | Invalid query parameter — typically a tampered or malformed cursor (&#x60;code: invalid_cursor&#x60;), out-of-range limit (&#x60;code: invalid_limit&#x60;), or &#x60;kind&#x60; not in {user, service-identity} (&#x60;code: invalid_kind&#x60;).  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the &#x60;read&#x60; ReBAC relation on the addressed Domain (body is a &#x60;PermissionDenied&#x60; problem) OR the pagination cursor was minted by a different caller and the per-(caller, pepper) HMAC binding rejected the replay (body is a &#x60;Problem&#x60; with &#x60;code &#x3D; cursor_binding_mismatch&#x60;).  |  -  |
**404** | Domain not found. Surfaced only after the authorisation gate has passed so the endpoint cannot be used as a Domain-id oracle.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **list_invitations**
> InvitationList list_invitations(id, cursor=cursor, limit=limit, status=status)

List Invitations on a Domain.

Returns a cursor-paginated page of Invitations attached to the
Domain identified by `{id}`, optionally filtered by status
. The handler runs the `read` ReBAC check on
`domain:<id>` BEFORE the persistence read so an unauthorised
caller receives `403` without the existence side-channel a
"load-then-check" flow would leak. The optional `status` query
parameter narrows the page to a single lifecycle state; the
special value `all` surfaces every status in one window — omit
to default to `all`.

The pagination cursor is HMAC-signed and bound to the
per-(caller, pepper) pseudonym, so a cursor minted by one
principal cannot be replayed by another — the cross-caller
replay surfaces as `403 cursor_binding_mismatch`. A tampered
envelope or unknown version byte stays on `400 invalid_cursor`.

The handler emits one `invitation.list` audit row per served
page.


### Example


```python
import plexsphere
from plexsphere.models.invitation_list import InvitationList
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
    api_instance = plexsphere.TenancyApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Domain identifier (UUIDv7). Bound on `/v1/domains/{id}` for the tenancy CRUD surface. 
    cursor = 'cursor_example' # str | Opaque continuation token returned by a previous call's `next_cursor`. The encoding is HMAC-signed by the server so a tampered cursor surfaces as `400 invalid_cursor` .  (optional)
    limit = 50 # int | Maximum number of items to return in a single page. The handler clamps the value to [1, 200] before forwarding it to the service.  (optional) (default to 50)
    status = 'status_example' # str | Optional status filter. Values outside the closed set surface as `400 invalid_status`. Defaults to `all` .  (optional)

    try:
        # List Invitations on a Domain.
        api_response = api_instance.list_invitations(id, cursor=cursor, limit=limit, status=status)
        print("The response of TenancyApi->list_invitations:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling TenancyApi->list_invitations: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Domain identifier (UUIDv7). Bound on &#x60;/v1/domains/{id}&#x60; for the tenancy CRUD surface.  | 
 **cursor** | **str**| Opaque continuation token returned by a previous call&#39;s &#x60;next_cursor&#x60;. The encoding is HMAC-signed by the server so a tampered cursor surfaces as &#x60;400 invalid_cursor&#x60; .  | [optional] 
 **limit** | **int**| Maximum number of items to return in a single page. The handler clamps the value to [1, 200] before forwarding it to the service.  | [optional] [default to 50]
 **status** | **str**| Optional status filter. Values outside the closed set surface as &#x60;400 invalid_status&#x60;. Defaults to &#x60;all&#x60; .  | [optional] 

### Return type

[**InvitationList**](InvitationList.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Page of Invitations. |  -  |
**400** | Invalid query parameter — &#x60;code&#x60; ∈ { &#x60;invalid_cursor&#x60;, &#x60;invalid_limit&#x60;, &#x60;invalid_status&#x60; } .  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the &#x60;read&#x60; ReBAC relation on the addressed Domain (body is a &#x60;PermissionDenied&#x60; problem) OR the pagination cursor was minted by a different caller and the per-(caller, pepper) HMAC binding rejected the replay (body is a &#x60;Problem&#x60; with &#x60;code &#x3D; cursor_binding_mismatch&#x60;).  |  -  |
**404** | Domain not found. Surfaced only after the authorisation gate has passed.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **list_projects**
> ProjectList list_projects(cursor=cursor, limit=limit, domain_id=domain_id)

List tenancy Projects.

Returns a slug-ordered page of tenancy Projects the caller is
authorised to see. Per-row
visibility is layered on top of the page: rows the caller
cannot `read` are filtered out so the response items are a
subset of the persistence-level page. The optional `domain_id`
query parameter scopes the page to a single parent Domain.

The pagination cursor is HMAC-signed and bound to the
per-(caller, pepper) pseudonym, so a cursor minted by one
principal cannot be replayed by another — the cross-caller
replay surfaces as `403 cursor_binding_mismatch`. A tampered
envelope or unknown version byte stays on `400 invalid_cursor`.


### Example


```python
import plexsphere
from plexsphere.models.project_list import ProjectList
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
    api_instance = plexsphere.TenancyApi(api_client)
    cursor = 'cursor_example' # str | Opaque continuation token returned by a previous call's `next_cursor`. The encoding is HMAC-signed by the server so a tampered cursor surfaces as `400`.  (optional)
    limit = 50 # int | Maximum number of items to return in a single page. The handler clamps the value to [1, 200] before forwarding it to the service.  (optional) (default to 50)
    domain_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Optional filter scoping the page to a single parent Domain. Omit to page across every Domain the caller is authorised to see.  (optional)

    try:
        # List tenancy Projects.
        api_response = api_instance.list_projects(cursor=cursor, limit=limit, domain_id=domain_id)
        print("The response of TenancyApi->list_projects:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling TenancyApi->list_projects: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **cursor** | **str**| Opaque continuation token returned by a previous call&#39;s &#x60;next_cursor&#x60;. The encoding is HMAC-signed by the server so a tampered cursor surfaces as &#x60;400&#x60;.  | [optional] 
 **limit** | **int**| Maximum number of items to return in a single page. The handler clamps the value to [1, 200] before forwarding it to the service.  | [optional] [default to 50]
 **domain_id** | **UUID**| Optional filter scoping the page to a single parent Domain. Omit to page across every Domain the caller is authorised to see.  | [optional] 

### Return type

[**ProjectList**](ProjectList.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Page of Projects. |  -  |
**400** | Invalid query parameters — typically a tampered or malformed cursor, an out-of-range limit, or a malformed &#x60;domain_id&#x60; filter.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to list Projects (body is a &#x60;PermissionDenied&#x60; problem) OR the pagination cursor was minted by a different caller and the per-(caller, pepper) HMAC binding rejected the replay (body is a &#x60;Problem&#x60; with &#x60;code &#x3D; cursor_binding_mismatch&#x60;).  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **patch_domain**
> DomainResponse patch_domain(id, domain_patch_request)

Patch mutable fields on a Domain.

Patches the Domain identified by `{id}`.
The body MUST set at least one of `name`, `description`,
`mesh_cidr`, or `reachability` — an empty body surfaces as
`400 empty_patch`.

DECISION: `slug` is intentionally NOT a patchable field. The
slug is the URL handle exported into cached dashboard links and
outbox projections; allowing it to mutate would silently rot
every cached reference. The handler rejects any body that
carries a `slug` key (even with the same value) at decode time
with `400 slug_immutable`. See the `tenancy` tag description
for the full rationale.

Retargeting `mesh_cidr` triggers two extra checks beyond the
aggregate invariant: the SQL GIST exclusion still polices
cross-Domain non-overlap (`409 mesh_cidr_overlap`), and the
service-level containment guard refuses a retarget that would
orphan an existing `project_mesh_ip_reservations.sub_range`
(`422 mesh_cidr_invalidates_subrange`, with the offending
`project_id` in the Problem detail).


### Example


```python
import plexsphere
from plexsphere.models.domain_patch_request import DomainPatchRequest
from plexsphere.models.domain_response import DomainResponse
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
    api_instance = plexsphere.TenancyApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Domain identifier (UUIDv7). Bound on `/v1/domains/{id}` for the tenancy CRUD surface. 
    domain_patch_request = {"description":"Acme Corp production — Q3 reorg.","mesh_cidr":"10.43.0.0/16"} # DomainPatchRequest | 

    try:
        # Patch mutable fields on a Domain.
        api_response = api_instance.patch_domain(id, domain_patch_request)
        print("The response of TenancyApi->patch_domain:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling TenancyApi->patch_domain: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Domain identifier (UUIDv7). Bound on &#x60;/v1/domains/{id}&#x60; for the tenancy CRUD surface.  | 
 **domain_patch_request** | [**DomainPatchRequest**](DomainPatchRequest.md)|  | 

### Return type

[**DomainResponse**](DomainResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Domain patched. |  -  |
**400** | Invalid body. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;invalid_domain&#x60;, &#x60;invalid_reachability_policy&#x60;, &#x60;slug_immutable&#x60;, &#x60;empty_patch&#x60; }.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to manage the addressed Domain. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Domain not found. Body is a &#x60;Problem&#x60; with &#x60;code: domain_not_found&#x60;.  |  -  |
**409** | mesh_cidr retarget collides with another Domain&#39;s mesh pool. Body is a &#x60;Problem&#x60; with &#x60;code: mesh_cidr_overlap&#x60; .  |  -  |
**413** | Request body exceeded the 8 KiB tenancy ceiling .  |  -  |
**422** | Mesh CIDR retarget would orphan an existing project sub-range. Body is a &#x60;Problem&#x60; with &#x60;code: mesh_cidr_invalidates_subrange&#x60; carrying the offending &#x60;project_id&#x60; in &#x60;detail&#x60;.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **patch_project**
> ProjectResponse patch_project(id, project_patch_request)

Patch mutable fields on a Project.

Patches the Project identified by `{id}`.
The body MUST set at least one of `name`, `description`,
`sub_range_cidr`, or `release_sub_range` — an empty body
surfaces as `400 empty_patch`.

DECISION: `slug` is intentionally NOT a patchable field — it
is the URL handle exported into cached dashboard links and
outbox projections. The handler rejects any body that carries
a `slug` key (even with the same value) at decode time with
`400 slug_immutable`.

Retargeting `sub_range_cidr` triggers an in-tx
sibling-overlap guard inside the parent Domain. A patch that
would overlap a sibling Project's reservation surfaces as
`422 sub_range_invalidates_allocation` carrying the offending
`project_id` and `sub_range` in the Problem detail.


### Example


```python
import plexsphere
from plexsphere.models.project_patch_request import ProjectPatchRequest
from plexsphere.models.project_response import ProjectResponse
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
    api_instance = plexsphere.TenancyApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Project identifier (UUIDv7). Bound on `/v1/projects/{id}` for the tenancy CRUD surface and on `/v1/projects/{id}/credentials` for the operator-facing OpenBao Credential Broker inventory list. 
    project_patch_request = {"description":"Acme Web — Q3 reorg.","sub_range_cidr":"10.42.8.0/22"} # ProjectPatchRequest | 

    try:
        # Patch mutable fields on a Project.
        api_response = api_instance.patch_project(id, project_patch_request)
        print("The response of TenancyApi->patch_project:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling TenancyApi->patch_project: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Project identifier (UUIDv7). Bound on &#x60;/v1/projects/{id}&#x60; for the tenancy CRUD surface and on &#x60;/v1/projects/{id}/credentials&#x60; for the operator-facing OpenBao Credential Broker inventory list.  | 
 **project_patch_request** | [**ProjectPatchRequest**](ProjectPatchRequest.md)|  | 

### Return type

[**ProjectResponse**](ProjectResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Project patched. |  -  |
**400** | Invalid body. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;invalid_project&#x60;, &#x60;slug_immutable&#x60;, &#x60;empty_patch&#x60;, &#x60;invalid_body&#x60;, &#x60;invalid_project_id&#x60; }.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to manage the addressed Project. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Project not found. Body is a &#x60;Problem&#x60; with &#x60;code: project_not_found&#x60;.  |  -  |
**409** | sub_range_cidr retarget collides with another Project&#39;s reservation in the same Domain. Body is a &#x60;Problem&#x60; with &#x60;code: sub_range_overlap&#x60;.  |  -  |
**413** | Request body exceeded the 8 KiB tenancy ceiling .  |  -  |
**422** | sub_range_cidr retarget would invalidate an existing allocation inside the parent Domain. Body is a &#x60;Problem&#x60; with &#x60;code: sub_range_invalidates_allocation&#x60; carrying the offending &#x60;project_id&#x60; and &#x60;sub_range&#x60; in &#x60;detail&#x60; .  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **revoke_invitation**
> revoke_invitation(id, invitation_id)

Revoke a pending Invitation.

Revokes the Invitation identified by `{invitationId}` inside
the Domain identified by `{id}`. The
handler authorises the call against the parent Domain's
`manage` ReBAC relation BEFORE invoking the service so an
unauthorised caller never produces an `InvitationRevoked`
outbox row.

Idempotent on terminal states — the aggregate's monotonic
status walk surfaces as:

  * `409 invitation_already_accepted` if the row is already
    accepted;
  * `409 invitation_already_expired` if the row is already
    expired (or the expiry sweeper has already flipped it).

A second revoke on a row that is already revoked is treated
as a no-op (`204`). A successful revoke emits an
`invitation.revoke` audit row and appends an
`InvitationRevoked` outbox event in the same transaction
.


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
    api_instance = plexsphere.TenancyApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Domain identifier (UUIDv7). Bound on `/v1/domains/{id}` for the tenancy CRUD surface. 
    invitation_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Invitation identifier (UUIDv7). Bound on `/v1/domains/{id}/invitations/{invitationId}` for the per- Domain invitation read / revoke surface. 

    try:
        # Revoke a pending Invitation.
        api_instance.revoke_invitation(id, invitation_id)
    except Exception as e:
        print("Exception when calling TenancyApi->revoke_invitation: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Domain identifier (UUIDv7). Bound on &#x60;/v1/domains/{id}&#x60; for the tenancy CRUD surface.  | 
 **invitation_id** | **UUID**| Invitation identifier (UUIDv7). Bound on &#x60;/v1/domains/{id}/invitations/{invitationId}&#x60; for the per- Domain invitation read / revoke surface.  | 

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
**204** | Invitation revoked (or was already revoked). |  -  |
**400** | Path &#x60;{invitationId}&#x60; was not a non-zero UUID. Body is a &#x60;Problem&#x60; with &#x60;code: invalid_invitation_id&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the &#x60;manage&#x60; ReBAC relation on the addressed Domain. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Invitation does not exist or belongs to a different Domain. Body is a &#x60;Problem&#x60; with &#x60;code: invitation_not_found&#x60;.  |  -  |
**409** | Target invitation is already in an absorbing terminal state that revoke cannot consume. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;invitation_already_accepted&#x60;, &#x60;invitation_already_expired&#x60; }.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

