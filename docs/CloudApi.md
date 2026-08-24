# plexsphere.CloudApi

All URIs are relative to *http://localhost*

Method | HTTP request | Description
------------- | ------------- | -------------
[**attach_cloud_credential_cloud**](CloudApi.md#attach_cloud_credential_cloud) | **POST** /v1/cloud-credentials/{id}/clouds | Attach a usage Cloud to a Cloud Credential.
[**create_cloud**](CloudApi.md#create_cloud) | **POST** /v1/clouds | Create a Cloud Inventory entry.
[**create_provider_bundle**](CloudApi.md#create_provider_bundle) | **POST** /v1/provider-bundles | Create a provider bundle.
[**delete_cloud**](CloudApi.md#delete_cloud) | **DELETE** /v1/clouds/{id} | Delete a Cloud.
[**delete_provider_bundle**](CloudApi.md#delete_provider_bundle) | **DELETE** /v1/provider-bundles/{id} | Delete a provider bundle.
[**detach_cloud_credential_cloud**](CloudApi.md#detach_cloud_credential_cloud) | **DELETE** /v1/cloud-credentials/{id}/clouds/{cloud_id} | Detach a usage Cloud from a Cloud Credential.
[**get_cloud**](CloudApi.md#get_cloud) | **GET** /v1/clouds/{id} | Fetch a Cloud by identifier.
[**get_cloud_credential**](CloudApi.md#get_cloud_credential) | **GET** /v1/cloud-credentials/{id} | Fetch a Cloud Credential&#39;s lifecycle metadata.
[**get_provider_bundle**](CloudApi.md#get_provider_bundle) | **GET** /v1/provider-bundles/{id} | Fetch a provider bundle by identifier.
[**grant_cloud_assignment**](CloudApi.md#grant_cloud_assignment) | **POST** /v1/clouds/{id}/cloud-assignments | Grant a Cloud to a Project (operator push).
[**grant_credential_assignment**](CloudApi.md#grant_credential_assignment) | **POST** /v1/cloud-credentials/{id}/credential-assignments | Grant a Cloud Credential to a Project (owner push).
[**issue_cloud_credential**](CloudApi.md#issue_cloud_credential) | **POST** /v1/clouds/{id}/cloud-credentials | Issue a new Cloud Credential under a Cloud.
[**list_cloud_assignments**](CloudApi.md#list_cloud_assignments) | **GET** /v1/projects/{id}/cloud-assignments | List the Cloud Assignments owned by a Project.
[**list_cloud_credential_clouds**](CloudApi.md#list_cloud_credential_clouds) | **GET** /v1/cloud-credentials/{id}/clouds | List the Clouds a Cloud Credential serves.
[**list_cloud_credentials**](CloudApi.md#list_cloud_credentials) | **GET** /v1/clouds/{id}/cloud-credentials | List Cloud Credentials owned by a Cloud.
[**list_clouds**](CloudApi.md#list_clouds) | **GET** /v1/clouds | List Cloud Inventory entries.
[**list_credential_assignments**](CloudApi.md#list_credential_assignments) | **GET** /v1/projects/{id}/credential-assignments | List the Credential Assignments owned by a Project.
[**list_provider_bundle_clouds**](CloudApi.md#list_provider_bundle_clouds) | **GET** /v1/provider-bundles/{id}/clouds | List the Clouds that reference a provider bundle.
[**list_provider_bundle_versions**](CloudApi.md#list_provider_bundle_versions) | **GET** /v1/provider-bundles/{id}/versions | List the published versions of a provider bundle.
[**list_provider_bundles**](CloudApi.md#list_provider_bundles) | **GET** /v1/provider-bundles | List provider bundles.
[**patch_cloud**](CloudApi.md#patch_cloud) | **PATCH** /v1/clouds/{id} | Patch mutable fields on a Cloud.
[**patch_provider_bundle**](CloudApi.md#patch_provider_bundle) | **PATCH** /v1/provider-bundles/{id} | Patch mutable fields on a provider bundle.
[**request_cloud_assignment**](CloudApi.md#request_cloud_assignment) | **POST** /v1/projects/{id}/cloud-assignments | Request usage of a Cloud for a Project.
[**request_credential_assignment**](CloudApi.md#request_credential_assignment) | **POST** /v1/projects/{id}/credential-assignments | Request a Credential Assignment for a Project.
[**revoke_cloud_assignment**](CloudApi.md#revoke_cloud_assignment) | **POST** /v1/cloud-assignments/{id}/revoke | Revoke a Cloud Assignment.
[**revoke_cloud_credential**](CloudApi.md#revoke_cloud_credential) | **POST** /v1/cloud-credentials/{id}/revoke | Revoke a Cloud Credential.
[**revoke_credential_assignment**](CloudApi.md#revoke_credential_assignment) | **POST** /v1/credential-assignments/{id}/revoke | Revoke a Credential Assignment.


# **attach_cloud_credential_cloud**
> CloudUsageRef attach_cloud_credential_cloud(id, cloud_credential_attach_request)

Attach a usage Cloud to a Cloud Credential.

Adds a usage edge so the Cloud Credential identified by `{id}`
additionally serves the Cloud named in the body. The handler
runs a `manage` ReBAC check on the credential's home Cloud
BEFORE decoding the body, then — once the body names the target
usage Cloud — a second `manage` check on that target Cloud, and
only then delegates to the Cloud Credentials Custodian which
records the usage edge. The caller must administer both Clouds:
the home Cloud whose credential is mutated and the target Cloud
that will start serving it.

Attach is idempotent: re-attaching an already-attached Cloud
returns `201` without creating a second edge. A revoked
credential cannot pick up further usage Clouds and is refused
with `409 cloud_credential_revoked`; a body `cloud_id` that
names no existing Cloud is refused with `404 cloud_not_found`.


### Example

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

```python
import plexsphere
from plexsphere.models.cloud_credential_attach_request import CloudCredentialAttachRequest
from plexsphere.models.cloud_usage_ref import CloudUsageRef
from plexsphere.rest import ApiException
from pprint import pprint

# Defining the host is optional and defaults to http://localhost
# See configuration.py for a list of all supported configuration parameters.
configuration = plexsphere.Configuration(
    host = "http://localhost"
)

# The client must configure the authentication and authorization parameters
# in accordance with the API server security policy.
# Examples for each auth method are provided below, use the example that
# satisfies your auth use case.

# Configure Bearer authorization (JWT): operatorBearer
configuration = plexsphere.Configuration(
    access_token = os.environ["BEARER_TOKEN"]
)

# Configure API key authorization: sessionCookie
configuration.api_key['sessionCookie'] = os.environ["API_KEY"]

# Uncomment below to setup prefix (e.g. Bearer) for API key, if needed
# configuration.api_key_prefix['sessionCookie'] = 'Bearer'

# Enter a context with an instance of the API client
with plexsphere.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = plexsphere.CloudApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Cloud Credential identifier (UUIDv7). Bound on `/v1/cloud-credentials/{id}`, `/v1/cloud-credentials/{id}/revoke`, `/v1/cloud-credentials/{id}/clouds`, `/v1/cloud-credentials/{id}/credential-assignments`, and `/v1/cloud-credentials/{id}/clouds/{cloud_id}` for the operator-facing Cloud Credentials read, revoke, and usage-Cloud attach/detach surface. 
    cloud_credential_attach_request = {"cloud_id":"0190a8b8-a0c0-7a0a-8a0a-a0a0a0a0a0c2"} # CloudCredentialAttachRequest | 

    try:
        # Attach a usage Cloud to a Cloud Credential.
        api_response = api_instance.attach_cloud_credential_cloud(id, cloud_credential_attach_request)
        print("The response of CloudApi->attach_cloud_credential_cloud:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling CloudApi->attach_cloud_credential_cloud: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Cloud Credential identifier (UUIDv7). Bound on &#x60;/v1/cloud-credentials/{id}&#x60;, &#x60;/v1/cloud-credentials/{id}/revoke&#x60;, &#x60;/v1/cloud-credentials/{id}/clouds&#x60;, &#x60;/v1/cloud-credentials/{id}/credential-assignments&#x60;, and &#x60;/v1/cloud-credentials/{id}/clouds/{cloud_id}&#x60; for the operator-facing Cloud Credentials read, revoke, and usage-Cloud attach/detach surface.  | 
 **cloud_credential_attach_request** | [**CloudCredentialAttachRequest**](CloudCredentialAttachRequest.md)|  | 

### Return type

[**CloudUsageRef**](CloudUsageRef.md)

### Authorization

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**201** | Usage Cloud attached (or already attached — the call is idempotent).  |  -  |
**400** | Malformed id or body. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;invalid_cloud_credential_id&#x60;, &#x60;invalid_body&#x60;, &#x60;invalid_cloud_id&#x60; }.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to manage the credential&#39;s home Cloud or the target usage Cloud. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Cloud Credential not found (&#x60;code: cloud_credential_not_found&#x60;) or the body &#x60;cloud_id&#x60; names no existing Cloud (&#x60;code: cloud_not_found&#x60;).  |  -  |
**409** | The Cloud Credential is revoked and cannot pick up further usage Clouds. Body is a &#x60;Problem&#x60; with &#x60;code: cloud_credential_revoked&#x60;.  |  -  |
**413** | Request body exceeded the 8 KiB Cloud Credentials ceiling.  |  -  |
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

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

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

# The client must configure the authentication and authorization parameters
# in accordance with the API server security policy.
# Examples for each auth method are provided below, use the example that
# satisfies your auth use case.

# Configure Bearer authorization (JWT): operatorBearer
configuration = plexsphere.Configuration(
    access_token = os.environ["BEARER_TOKEN"]
)

# Configure API key authorization: sessionCookie
configuration.api_key['sessionCookie'] = os.environ["API_KEY"]

# Uncomment below to setup prefix (e.g. Bearer) for API key, if needed
# configuration.api_key_prefix['sessionCookie'] = 'Bearer'

# Enter a context with an instance of the API client
with plexsphere.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = plexsphere.CloudApi(api_client)
    cloud_create_request = {"display_name":"Acme AWS Production","slug":"acme-aws-prod","provider":"aws","endpoint":{"region":"us-east-1","role_arn":"arn:aws:iam::123456789012:role/PlexsphereProvisioner"},"region_defaults":{"vpc_cidr":"10.42.0.0/16"},"external_id":"123456789012","provider_packages":[{"source":"xpkg.upbound.io/upbound/provider-aws-ec2","version":"v2.6.1"},{"source":"xpkg.upbound.io/upbound/provider-aws-s3","version":"v2.6.1"}],"provider_config_api_version":"aws.m.upbound.io/v1beta1"} # CloudCreateRequest | 

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

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**201** | Cloud created. |  * Location - Canonical read URL of the created route — &#x60;/v1/telemetry-routes/{id}&#x60;.  <br>  |
**400** | Aggregate or validator rejected the body — empty &#x60;display_name&#x60;, malformed &#x60;slug&#x60;, malformed &#x60;endpoint&#x60; or &#x60;region_defaults&#x60;, unknown &#x60;provider&#x60;, per-provider field-level validator rejection, or a provider-mode admission failure. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;invalid_cloud&#x60;, &#x60;unknown_provider&#x60;, &#x60;invalid_cloud_endpoint&#x60;, &#x60;invalid_cloud_region_defaults&#x60;, &#x60;invalid_cloud_provider_mode&#x60;, &#x60;unknown_provider_bundle&#x60;, &#x60;provider_bundle_provider_mismatch&#x60;, &#x60;invalid_body&#x60; }.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to create Clouds. Body is a &#x60;PermissionDenied&#x60; problem carrying the ReBAC denial &#x60;reason&#x60;, &#x60;relation_path&#x60;, and &#x60;correlation_id&#x60;.  |  -  |
**409** | Conflict — the proposed Cloud collides with a persisted row. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;cloud_slug_conflict&#x60;, &#x60;cloud_external_id_conflict&#x60; }.  |  -  |
**413** | Request body exceeded the 8 KiB Cloud Inventory ceiling enforced by the handler.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **create_provider_bundle**
> ProviderBundleResponse create_provider_bundle(provider_bundle_create_request)

Create a provider bundle.

Creates a new `ProviderBundle` aggregate — one reusable
Crossplane provider-package declaration, authored once and
addressed by a stable handle so a Cloud points at it instead of
repeating the same packages inline. The aggregate enforces
every issuance invariant — non-empty `display_name`, kebab-case
`slug`, closed-enum `provider`, at least one provider package
with each `source` named at most once, and a `<group>/<version>`
`provider_config_api_version`. An invariant rejection surfaces
as `400 invalid_provider_bundle`; a duplicate slug surfaces as
`409 provider_bundle_slug_conflict`.

On success the handler emits a `provider_bundle.create` audit
row and appends a `ProviderBundleCreated` outbox event in the
same transaction.

The ReBAC grants that make the new bundle readable — the
`platform` parent edge and the creating principal's
`bundle_admin` grant — are written by the authz-sync consumer
draining that outbox row, not by this request. The `201` is
therefore ahead of the graph: until the consumer has drained,
`GET /v1/provider-bundles/{id}` on the bundle just created
answers `403` and `GET /v1/provider-bundles` omits the row. A
client that reads back immediately should retry on `403` rather
than treat it as a permanent denial.


### Example

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

```python
import plexsphere
from plexsphere.models.provider_bundle_create_request import ProviderBundleCreateRequest
from plexsphere.models.provider_bundle_response import ProviderBundleResponse
from plexsphere.rest import ApiException
from pprint import pprint

# Defining the host is optional and defaults to http://localhost
# See configuration.py for a list of all supported configuration parameters.
configuration = plexsphere.Configuration(
    host = "http://localhost"
)

# The client must configure the authentication and authorization parameters
# in accordance with the API server security policy.
# Examples for each auth method are provided below, use the example that
# satisfies your auth use case.

# Configure Bearer authorization (JWT): operatorBearer
configuration = plexsphere.Configuration(
    access_token = os.environ["BEARER_TOKEN"]
)

# Configure API key authorization: sessionCookie
configuration.api_key['sessionCookie'] = os.environ["API_KEY"]

# Uncomment below to setup prefix (e.g. Bearer) for API key, if needed
# configuration.api_key_prefix['sessionCookie'] = 'Bearer'

# Enter a context with an instance of the API client
with plexsphere.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = plexsphere.CloudApi(api_client)
    provider_bundle_create_request = {"display_name":"AWS baseline packages","slug":"aws-baseline","provider":"aws","provider_packages":[{"source":"xpkg.upbound.io/upbound/provider-aws-ec2","version":"v2.6.1"},{"source":"xpkg.upbound.io/upbound/provider-aws-s3","version":"v2.6.1"}],"provider_config_api_version":"aws.m.upbound.io/v1beta1"} # ProviderBundleCreateRequest | 

    try:
        # Create a provider bundle.
        api_response = api_instance.create_provider_bundle(provider_bundle_create_request)
        print("The response of CloudApi->create_provider_bundle:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling CloudApi->create_provider_bundle: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **provider_bundle_create_request** | [**ProviderBundleCreateRequest**](ProviderBundleCreateRequest.md)|  | 

### Return type

[**ProviderBundleResponse**](ProviderBundleResponse.md)

### Authorization

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**201** | Provider bundle created. |  * Location - Canonical read URL of the created route — &#x60;/v1/telemetry-routes/{id}&#x60;.  <br>  |
**400** | The aggregate rejected the body — empty &#x60;display_name&#x60;, malformed &#x60;slug&#x60;, unknown &#x60;provider&#x60;, an empty package set or one naming the same &#x60;source&#x60; twice, or a malformed &#x60;provider_config_api_version&#x60;. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;invalid_provider_bundle&#x60;, &#x60;invalid_body&#x60; }.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to create provider bundles. Body is a &#x60;PermissionDenied&#x60; problem carrying the ReBAC denial &#x60;reason&#x60;, &#x60;relation_path&#x60;, and &#x60;correlation_id&#x60;.  |  -  |
**409** | Conflict — the proposed bundle collides with a persisted row. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;provider_bundle_slug_conflict&#x60;, &#x60;provider_bundle_conflict&#x60; }.  |  -  |
**413** | Request body exceeded the 8 KiB Cloud Inventory ceiling enforced by the handler. Body is a &#x60;Problem&#x60; with &#x60;code: request_body_too_large&#x60;.  |  -  |
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

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

```python
import plexsphere
from plexsphere.rest import ApiException
from pprint import pprint

# Defining the host is optional and defaults to http://localhost
# See configuration.py for a list of all supported configuration parameters.
configuration = plexsphere.Configuration(
    host = "http://localhost"
)

# The client must configure the authentication and authorization parameters
# in accordance with the API server security policy.
# Examples for each auth method are provided below, use the example that
# satisfies your auth use case.

# Configure Bearer authorization (JWT): operatorBearer
configuration = plexsphere.Configuration(
    access_token = os.environ["BEARER_TOKEN"]
)

# Configure API key authorization: sessionCookie
configuration.api_key['sessionCookie'] = os.environ["API_KEY"]

# Uncomment below to setup prefix (e.g. Bearer) for API key, if needed
# configuration.api_key_prefix['sessionCookie'] = 'Bearer'

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

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**204** | Cloud deleted. |  -  |
**400** | The path &#x60;{id}&#x60; is not a non-zero UUID. Body is a &#x60;Problem&#x60; with &#x60;code: invalid_cloud_id&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to delete the addressed Cloud. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Cloud not found. Body is a &#x60;Problem&#x60; with &#x60;code: cloud_not_found&#x60;.  |  -  |
**409** | Cloud still owns at least one child aggregate. Body is a &#x60;Problem&#x60; with &#x60;code: cloud_not_empty&#x60; and the optional &#x60;cloud_child_counts&#x60; extension carrying the structured &#x60;CloudChildCounts&#x60; so the operator knows how many credentials are still attached.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **delete_provider_bundle**
> delete_provider_bundle(id)

Delete a provider bundle.

Deletes the `ProviderBundle` identified by `{id}`. The
referenced-bundle guard runs inside the same transaction as the
row delete: at least one Cloud that still takes its provider
configuration from the bundle forces
`409 provider_bundle_referenced` with the `referencing_clouds`
count in the Problem body, so the operator knows how many
Clouds to re-point before retrying. A Cloud that starts
referencing the bundle between the guard's count and the
DELETE is caught by defense-in-depth — the foreign-key
violation surfaces as the same `409`, without the count.


### Example

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

```python
import plexsphere
from plexsphere.rest import ApiException
from pprint import pprint

# Defining the host is optional and defaults to http://localhost
# See configuration.py for a list of all supported configuration parameters.
configuration = plexsphere.Configuration(
    host = "http://localhost"
)

# The client must configure the authentication and authorization parameters
# in accordance with the API server security policy.
# Examples for each auth method are provided below, use the example that
# satisfies your auth use case.

# Configure Bearer authorization (JWT): operatorBearer
configuration = plexsphere.Configuration(
    access_token = os.environ["BEARER_TOKEN"]
)

# Configure API key authorization: sessionCookie
configuration.api_key['sessionCookie'] = os.environ["API_KEY"]

# Uncomment below to setup prefix (e.g. Bearer) for API key, if needed
# configuration.api_key_prefix['sessionCookie'] = 'Bearer'

# Enter a context with an instance of the API client
with plexsphere.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = plexsphere.CloudApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Provider bundle identifier (UUIDv7). Bound on `/v1/provider-bundles/{id}` for the Cloud Inventory provider-bundle CRUD surface, and on `/v1/provider-bundles/{id}/clouds` for the roster of Clouds that reference the bundle. 

    try:
        # Delete a provider bundle.
        api_instance.delete_provider_bundle(id)
    except Exception as e:
        print("Exception when calling CloudApi->delete_provider_bundle: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Provider bundle identifier (UUIDv7). Bound on &#x60;/v1/provider-bundles/{id}&#x60; for the Cloud Inventory provider-bundle CRUD surface, and on &#x60;/v1/provider-bundles/{id}/clouds&#x60; for the roster of Clouds that reference the bundle.  | 

### Return type

void (empty response body)

### Authorization

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**204** | Provider bundle deleted. |  -  |
**400** | The path &#x60;{id}&#x60; is not a non-zero UUID. Body is a &#x60;Problem&#x60; with &#x60;code: invalid_provider_bundle_id&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to delete the addressed provider bundle. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Provider bundle not found. Body is a &#x60;Problem&#x60; with &#x60;code: provider_bundle_not_found&#x60;.  |  -  |
**409** | At least one Cloud still takes its provider configuration from the bundle. Body is a &#x60;Problem&#x60; with &#x60;code: provider_bundle_referenced&#x60; and the optional &#x60;referencing_clouds&#x60; extension carrying how many Clouds reference it. The extension is absent on the racing arm, where the foreign-key violation reaches the same code without a count.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **detach_cloud_credential_cloud**
> detach_cloud_credential_cloud(id, cloud_id)

Detach a usage Cloud from a Cloud Credential.

Removes the usage edge that binds the Cloud Credential
identified by `{id}` to the usage Cloud `{cloud_id}`. The handler
authorises the detach against a `manage` ReBAC check on EITHER
the credential's home Cloud OR the target usage Cloud — so the
target Cloud's owner can remove an edge attached to their Cloud —
then delegates to the Cloud Credentials Custodian.

Detach is idempotent: detaching an absent edge returns `204`.
The credential's home Cloud anchors the KV-v2 path and can never
be detached — a detach targeting it is refused with
`409 cannot_detach_home_cloud`.


### Example

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

```python
import plexsphere
from plexsphere.rest import ApiException
from pprint import pprint

# Defining the host is optional and defaults to http://localhost
# See configuration.py for a list of all supported configuration parameters.
configuration = plexsphere.Configuration(
    host = "http://localhost"
)

# The client must configure the authentication and authorization parameters
# in accordance with the API server security policy.
# Examples for each auth method are provided below, use the example that
# satisfies your auth use case.

# Configure Bearer authorization (JWT): operatorBearer
configuration = plexsphere.Configuration(
    access_token = os.environ["BEARER_TOKEN"]
)

# Configure API key authorization: sessionCookie
configuration.api_key['sessionCookie'] = os.environ["API_KEY"]

# Uncomment below to setup prefix (e.g. Bearer) for API key, if needed
# configuration.api_key_prefix['sessionCookie'] = 'Bearer'

# Enter a context with an instance of the API client
with plexsphere.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = plexsphere.CloudApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Cloud Credential identifier (UUIDv7). Bound on `/v1/cloud-credentials/{id}`, `/v1/cloud-credentials/{id}/revoke`, `/v1/cloud-credentials/{id}/clouds`, `/v1/cloud-credentials/{id}/credential-assignments`, and `/v1/cloud-credentials/{id}/clouds/{cloud_id}` for the operator-facing Cloud Credentials read, revoke, and usage-Cloud attach/detach surface. 
    cloud_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Identifier of the usage Cloud to detach (UUIDv7). Must be a non-zero UUID — a malformed value is rejected with `400 invalid_cloud_id`. 

    try:
        # Detach a usage Cloud from a Cloud Credential.
        api_instance.detach_cloud_credential_cloud(id, cloud_id)
    except Exception as e:
        print("Exception when calling CloudApi->detach_cloud_credential_cloud: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Cloud Credential identifier (UUIDv7). Bound on &#x60;/v1/cloud-credentials/{id}&#x60;, &#x60;/v1/cloud-credentials/{id}/revoke&#x60;, &#x60;/v1/cloud-credentials/{id}/clouds&#x60;, &#x60;/v1/cloud-credentials/{id}/credential-assignments&#x60;, and &#x60;/v1/cloud-credentials/{id}/clouds/{cloud_id}&#x60; for the operator-facing Cloud Credentials read, revoke, and usage-Cloud attach/detach surface.  | 
 **cloud_id** | **UUID**| Identifier of the usage Cloud to detach (UUIDv7). Must be a non-zero UUID — a malformed value is rejected with &#x60;400 invalid_cloud_id&#x60;.  | 

### Return type

void (empty response body)

### Authorization

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**204** | Usage Cloud detached (or already absent — the call is idempotent). No body.  |  -  |
**400** | Malformed id. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;invalid_cloud_credential_id&#x60;, &#x60;invalid_cloud_id&#x60; }.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to manage either the credential&#39;s home Cloud or the target usage Cloud. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Cloud Credential not found. Body is a &#x60;Problem&#x60; with &#x60;code: cloud_credential_not_found&#x60;.  |  -  |
**409** | The addressed Cloud is the credential&#39;s home Cloud, which is undetachable. Body is a &#x60;Problem&#x60; with &#x60;code: cannot_detach_home_cloud&#x60;.  |  -  |
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

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

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

# The client must configure the authentication and authorization parameters
# in accordance with the API server security policy.
# Examples for each auth method are provided below, use the example that
# satisfies your auth use case.

# Configure Bearer authorization (JWT): operatorBearer
configuration = plexsphere.Configuration(
    access_token = os.environ["BEARER_TOKEN"]
)

# Configure API key authorization: sessionCookie
configuration.api_key['sessionCookie'] = os.environ["API_KEY"]

# Uncomment below to setup prefix (e.g. Bearer) for API key, if needed
# configuration.api_key_prefix['sessionCookie'] = 'Bearer'

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

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Cloud found. |  -  |
**400** | The path &#x60;{id}&#x60; is not a non-zero UUID. Body is a &#x60;Problem&#x60; with &#x60;code: invalid_cloud_id&#x60;.  |  -  |
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

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

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

# The client must configure the authentication and authorization parameters
# in accordance with the API server security policy.
# Examples for each auth method are provided below, use the example that
# satisfies your auth use case.

# Configure Bearer authorization (JWT): operatorBearer
configuration = plexsphere.Configuration(
    access_token = os.environ["BEARER_TOKEN"]
)

# Configure API key authorization: sessionCookie
configuration.api_key['sessionCookie'] = os.environ["API_KEY"]

# Uncomment below to setup prefix (e.g. Bearer) for API key, if needed
# configuration.api_key_prefix['sessionCookie'] = 'Bearer'

# Enter a context with an instance of the API client
with plexsphere.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = plexsphere.CloudApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Cloud Credential identifier (UUIDv7). Bound on `/v1/cloud-credentials/{id}`, `/v1/cloud-credentials/{id}/revoke`, `/v1/cloud-credentials/{id}/clouds`, `/v1/cloud-credentials/{id}/credential-assignments`, and `/v1/cloud-credentials/{id}/clouds/{cloud_id}` for the operator-facing Cloud Credentials read, revoke, and usage-Cloud attach/detach surface. 

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
 **id** | **UUID**| Cloud Credential identifier (UUIDv7). Bound on &#x60;/v1/cloud-credentials/{id}&#x60;, &#x60;/v1/cloud-credentials/{id}/revoke&#x60;, &#x60;/v1/cloud-credentials/{id}/clouds&#x60;, &#x60;/v1/cloud-credentials/{id}/credential-assignments&#x60;, and &#x60;/v1/cloud-credentials/{id}/clouds/{cloud_id}&#x60; for the operator-facing Cloud Credentials read, revoke, and usage-Cloud attach/detach surface.  | 

### Return type

[**CloudCredentialResponse**](CloudCredentialResponse.md)

### Authorization

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

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

# **get_provider_bundle**
> ProviderBundleResponse get_provider_bundle(id)

Fetch a provider bundle by identifier.

Returns the `ProviderBundle` identified by `{id}`. The handler
runs the `observe` ReBAC check BEFORE the persistence read, so
an unauthorised caller receives `403` without the existence
side-channel a "load-then-check" flow would leak. A missing
aggregate surfaces as `404 provider_bundle_not_found` — but
only for a caller the graph already grants `observe` on that
id. An id no bundle ever answered to carries no tuples at all,
so the gate denies it first and the caller reads `403`. That is
the same withholding the check is there to perform: `404` and
`403` are deliberately indistinguishable for an id the caller
has no grant on.


### Example

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

```python
import plexsphere
from plexsphere.models.provider_bundle_response import ProviderBundleResponse
from plexsphere.rest import ApiException
from pprint import pprint

# Defining the host is optional and defaults to http://localhost
# See configuration.py for a list of all supported configuration parameters.
configuration = plexsphere.Configuration(
    host = "http://localhost"
)

# The client must configure the authentication and authorization parameters
# in accordance with the API server security policy.
# Examples for each auth method are provided below, use the example that
# satisfies your auth use case.

# Configure Bearer authorization (JWT): operatorBearer
configuration = plexsphere.Configuration(
    access_token = os.environ["BEARER_TOKEN"]
)

# Configure API key authorization: sessionCookie
configuration.api_key['sessionCookie'] = os.environ["API_KEY"]

# Uncomment below to setup prefix (e.g. Bearer) for API key, if needed
# configuration.api_key_prefix['sessionCookie'] = 'Bearer'

# Enter a context with an instance of the API client
with plexsphere.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = plexsphere.CloudApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Provider bundle identifier (UUIDv7). Bound on `/v1/provider-bundles/{id}` for the Cloud Inventory provider-bundle CRUD surface, and on `/v1/provider-bundles/{id}/clouds` for the roster of Clouds that reference the bundle. 

    try:
        # Fetch a provider bundle by identifier.
        api_response = api_instance.get_provider_bundle(id)
        print("The response of CloudApi->get_provider_bundle:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling CloudApi->get_provider_bundle: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Provider bundle identifier (UUIDv7). Bound on &#x60;/v1/provider-bundles/{id}&#x60; for the Cloud Inventory provider-bundle CRUD surface, and on &#x60;/v1/provider-bundles/{id}/clouds&#x60; for the roster of Clouds that reference the bundle.  | 

### Return type

[**ProviderBundleResponse**](ProviderBundleResponse.md)

### Authorization

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Provider bundle found. |  -  |
**400** | The path &#x60;{id}&#x60; is not a non-zero UUID. Body is a &#x60;Problem&#x60; with &#x60;code: invalid_provider_bundle_id&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to read the addressed provider bundle. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Provider bundle not found. Body is a &#x60;Problem&#x60; with &#x60;code: provider_bundle_not_found&#x60;.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **grant_cloud_assignment**
> CloudAssignmentResponse grant_cloud_assignment(id, cloud_assignment_grant_request)

Grant a Cloud to a Project (operator push).

Assigns the Cloud identified by `{id}` to the Project named in
the body as a single authoritative Platform Operator action. The
handler runs an `assign` ReBAC check on the Cloud BEFORE the
persistence write, then delegates to the Cloud Assignment
application service which creates the assignment already approved
AND materialised in one step — the Cloud is immediately usable in
the Project — and appends a `CloudAssignmentGranted` outbox event
in a single transaction.

The operator grant bypasses the second-party approval rule by
design: there is no separate requester to compare against. A
second live assignment for the same (Project, Cloud) pair is
rejected with `409 duplicate_live_cloud_assignment`.


### Example

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

```python
import plexsphere
from plexsphere.models.cloud_assignment_grant_request import CloudAssignmentGrantRequest
from plexsphere.models.cloud_assignment_response import CloudAssignmentResponse
from plexsphere.rest import ApiException
from pprint import pprint

# Defining the host is optional and defaults to http://localhost
# See configuration.py for a list of all supported configuration parameters.
configuration = plexsphere.Configuration(
    host = "http://localhost"
)

# The client must configure the authentication and authorization parameters
# in accordance with the API server security policy.
# Examples for each auth method are provided below, use the example that
# satisfies your auth use case.

# Configure Bearer authorization (JWT): operatorBearer
configuration = plexsphere.Configuration(
    access_token = os.environ["BEARER_TOKEN"]
)

# Configure API key authorization: sessionCookie
configuration.api_key['sessionCookie'] = os.environ["API_KEY"]

# Uncomment below to setup prefix (e.g. Bearer) for API key, if needed
# configuration.api_key_prefix['sessionCookie'] = 'Bearer'

# Enter a context with an instance of the API client
with plexsphere.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = plexsphere.CloudApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Cloud identifier (UUIDv7). Bound on `/v1/clouds/{id}` for the Cloud Inventory CRUD surface. 
    cloud_assignment_grant_request = {"project_id":"0190a8b8-a0c0-7a0a-8a0a-a0a0a0a0a0c1"} # CloudAssignmentGrantRequest | 

    try:
        # Grant a Cloud to a Project (operator push).
        api_response = api_instance.grant_cloud_assignment(id, cloud_assignment_grant_request)
        print("The response of CloudApi->grant_cloud_assignment:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling CloudApi->grant_cloud_assignment: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Cloud identifier (UUIDv7). Bound on &#x60;/v1/clouds/{id}&#x60; for the Cloud Inventory CRUD surface.  | 
 **cloud_assignment_grant_request** | [**CloudAssignmentGrantRequest**](CloudAssignmentGrantRequest.md)|  | 

### Return type

[**CloudAssignmentResponse**](CloudAssignmentResponse.md)

### Authorization

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**201** | Cloud Assignment granted, already approved and materialised. No &#x60;Location&#x60; header is sent — the aggregate has no by-id read surface; poll it through the Cloud&#39;s assignment list.  |  -  |
**400** | Malformed id or body. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;invalid_cloud_id&#x60;, &#x60;invalid_body&#x60;, &#x60;invalid_project_id&#x60; }.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to assign the Cloud. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**409** | A live Cloud Assignment already exists for the same (Project, Cloud) pair. Body is a &#x60;Problem&#x60; with &#x60;code: duplicate_live_cloud_assignment&#x60;.  |  -  |
**413** | Request body exceeded the 8 KiB ceiling. Body is a &#x60;Problem&#x60; with &#x60;code: request_body_too_large&#x60;.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **grant_credential_assignment**
> CredentialAssignmentResponse grant_credential_assignment(id, credential_assignment_grant_request)

Grant a Cloud Credential to a Project (owner push).

Binds the Cloud Credential identified by `{id}` to the Project
named in the body as a single authoritative action. The handler
runs an `assign` ReBAC check on the credential BEFORE the
persistence write, then delegates to the Credential Assignment
application service which creates the assignment already approved
AND materialised in one step — the credential is immediately
usable in the Project — and appends a
`CredentialAssignmentGranted` outbox event in a single
transaction.

This is the owner-push counterpart to
`RequestCredentialAssignment`, which the consuming Project
initiates and a second party decides. The grant bypasses the
second-party approval rule by design: there is no separate
requester to compare against, which is also why it exists —
requesting and then approving one's own request is refused by
the self-approval guard, so without this route a credential's
owner could not place it at all. Mirrors `GrantCloudAssignment`
on the Cloud side.

The receiving Project must already be able to use the Cloud the
credential belongs to — an approved Cloud Assignment — otherwise
the grant is rejected with `422 cloud_not_usable_in_project`. A
credential is only usable where both assignments are in place, so
a grant without the Cloud Assignment would bind the credential and
still be refused at deploy time. A `{id}` naming no credential at
all is rejected with `422 credential_not_assignable`.

A second live assignment for the same (Project, Credential) pair
is rejected with `409 duplicate_live_assignment`.


### Example

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

```python
import plexsphere
from plexsphere.models.credential_assignment_grant_request import CredentialAssignmentGrantRequest
from plexsphere.models.credential_assignment_response import CredentialAssignmentResponse
from plexsphere.rest import ApiException
from pprint import pprint

# Defining the host is optional and defaults to http://localhost
# See configuration.py for a list of all supported configuration parameters.
configuration = plexsphere.Configuration(
    host = "http://localhost"
)

# The client must configure the authentication and authorization parameters
# in accordance with the API server security policy.
# Examples for each auth method are provided below, use the example that
# satisfies your auth use case.

# Configure Bearer authorization (JWT): operatorBearer
configuration = plexsphere.Configuration(
    access_token = os.environ["BEARER_TOKEN"]
)

# Configure API key authorization: sessionCookie
configuration.api_key['sessionCookie'] = os.environ["API_KEY"]

# Uncomment below to setup prefix (e.g. Bearer) for API key, if needed
# configuration.api_key_prefix['sessionCookie'] = 'Bearer'

# Enter a context with an instance of the API client
with plexsphere.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = plexsphere.CloudApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Cloud Credential identifier (UUIDv7). Bound on `/v1/cloud-credentials/{id}`, `/v1/cloud-credentials/{id}/revoke`, `/v1/cloud-credentials/{id}/clouds`, `/v1/cloud-credentials/{id}/credential-assignments`, and `/v1/cloud-credentials/{id}/clouds/{cloud_id}` for the operator-facing Cloud Credentials read, revoke, and usage-Cloud attach/detach surface. 
    credential_assignment_grant_request = {"project_id":"0190a8b8-a0c0-7a0a-8a0a-a0a0a0a0a0c1"} # CredentialAssignmentGrantRequest | 

    try:
        # Grant a Cloud Credential to a Project (owner push).
        api_response = api_instance.grant_credential_assignment(id, credential_assignment_grant_request)
        print("The response of CloudApi->grant_credential_assignment:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling CloudApi->grant_credential_assignment: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Cloud Credential identifier (UUIDv7). Bound on &#x60;/v1/cloud-credentials/{id}&#x60;, &#x60;/v1/cloud-credentials/{id}/revoke&#x60;, &#x60;/v1/cloud-credentials/{id}/clouds&#x60;, &#x60;/v1/cloud-credentials/{id}/credential-assignments&#x60;, and &#x60;/v1/cloud-credentials/{id}/clouds/{cloud_id}&#x60; for the operator-facing Cloud Credentials read, revoke, and usage-Cloud attach/detach surface.  | 
 **credential_assignment_grant_request** | [**CredentialAssignmentGrantRequest**](CredentialAssignmentGrantRequest.md)|  | 

### Return type

[**CredentialAssignmentResponse**](CredentialAssignmentResponse.md)

### Authorization

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**201** | Credential Assignment granted, already approved and materialised. No &#x60;Location&#x60; header is sent — the aggregate has no by-id read surface; poll it through the Project&#39;s assignment list.  |  -  |
**400** | Malformed id or body. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;invalid_cloud_credential_id&#x60;, &#x60;invalid_body&#x60;, &#x60;invalid_project_id&#x60; }.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to assign the Cloud Credential. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**409** | A live Credential Assignment already exists for the same (Project, Credential) pair. Body is a &#x60;Problem&#x60; with &#x60;code: duplicate_live_assignment&#x60;.  |  -  |
**413** | Request body exceeded the 8 KiB ceiling. Body is a &#x60;Problem&#x60; with &#x60;code: request_body_too_large&#x60;.  |  -  |
**422** | Semantic rejection. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;cloud_not_usable_in_project&#x60;, &#x60;credential_not_assignable&#x60; }. &#x60;cloud_not_usable_in_project&#x60; — the receiving Project holds no approved Cloud Assignment for the Cloud the credential belongs to, so the grant would bind a credential the Project still could not deploy with. &#x60;credential_not_assignable&#x60; — &#x60;{id}&#x60; names no Cloud Credential (a missing reference is unusable for the same reason a revoked one is).  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **issue_cloud_credential**
> CloudCredentialResponse issue_cloud_credential(id, cloud_credential_issue_request)

Issue a new Cloud Credential under a Cloud.

Issues a new Cloud Credential owned by the Cloud identified by
`{id}`. The handler runs a `manage` ReBAC check on the parent
Cloud BEFORE decoding the request body, then delegates to the
Cloud Credentials Custodian which writes the secret material to
OpenBao KV-v2, persists the broker row, and appends a
`CloudCredentialIssued` outbox event in a single transaction.

The response carries the metadata-only projection of the freshly
issued credential — the same shape the read and revoke surfaces
return. It NEVER echoes the payload, key-values, KV mount, KV
path, or KV version: the request material is accepted inbound
only and the storage location stays storage-internal.


### Example

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

```python
import plexsphere
from plexsphere.models.cloud_credential_issue_request import CloudCredentialIssueRequest
from plexsphere.models.cloud_credential_response import CloudCredentialResponse
from plexsphere.rest import ApiException
from pprint import pprint

# Defining the host is optional and defaults to http://localhost
# See configuration.py for a list of all supported configuration parameters.
configuration = plexsphere.Configuration(
    host = "http://localhost"
)

# The client must configure the authentication and authorization parameters
# in accordance with the API server security policy.
# Examples for each auth method are provided below, use the example that
# satisfies your auth use case.

# Configure Bearer authorization (JWT): operatorBearer
configuration = plexsphere.Configuration(
    access_token = os.environ["BEARER_TOKEN"]
)

# Configure API key authorization: sessionCookie
configuration.api_key['sessionCookie'] = os.environ["API_KEY"]

# Uncomment below to setup prefix (e.g. Bearer) for API key, if needed
# configuration.api_key_prefix['sessionCookie'] = 'Bearer'

# Enter a context with an instance of the API client
with plexsphere.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = plexsphere.CloudApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Cloud identifier (UUIDv7). Bound on `/v1/clouds/{id}` for the Cloud Inventory CRUD surface. 
    cloud_credential_issue_request = {"display_name":"Acme AWS provisioner secret","payload":"ZXhhbXBsZS1zZWNyZXQtYnl0ZXM=","key_values":{"role_arn":"arn:aws:iam::123456789012:role/acme-provisioner"}} # CloudCredentialIssueRequest | 

    try:
        # Issue a new Cloud Credential under a Cloud.
        api_response = api_instance.issue_cloud_credential(id, cloud_credential_issue_request)
        print("The response of CloudApi->issue_cloud_credential:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling CloudApi->issue_cloud_credential: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Cloud identifier (UUIDv7). Bound on &#x60;/v1/clouds/{id}&#x60; for the Cloud Inventory CRUD surface.  | 
 **cloud_credential_issue_request** | [**CloudCredentialIssueRequest**](CloudCredentialIssueRequest.md)|  | 

### Return type

[**CloudCredentialResponse**](CloudCredentialResponse.md)

### Authorization

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**201** | Cloud Credential issued. The &#x60;Location&#x60; header carries the canonical URL of the new credential.  |  * Location - Canonical read URL of the created route — &#x60;/v1/telemetry-routes/{id}&#x60;.  <br>  |
**400** | Malformed id or body. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;invalid_cloud_id&#x60;, &#x60;invalid_body&#x60;, &#x60;invalid_display_name&#x60;, &#x60;invalid_payload&#x60; }.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to manage the parent Cloud. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**413** | Request body exceeded the 8 KiB Cloud Credentials ceiling.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **list_cloud_assignments**
> CloudAssignmentList list_cloud_assignments(id, cursor=cursor, limit=limit)

List the Cloud Assignments owned by a Project.

Returns a creation-ordered page of Cloud Assignment lifecycle
metadata for the Project identified by `{id}`. The handler runs
a top-level `read` ReBAC check on the parent Project BEFORE the
persistence read; every assignment in the page belongs to the
one path Project, so the project `read` check authorises the
whole page.

The pagination cursor is HMAC-signed and bound to the
per-(caller, pepper) pseudonym, so a cursor minted by one
principal cannot be replayed by another — the cross-caller
replay surfaces as `403 cursor_binding_mismatch`. A tampered
envelope or unknown version byte stays on `400 invalid_cursor`.


### Example

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

```python
import plexsphere
from plexsphere.models.cloud_assignment_list import CloudAssignmentList
from plexsphere.rest import ApiException
from pprint import pprint

# Defining the host is optional and defaults to http://localhost
# See configuration.py for a list of all supported configuration parameters.
configuration = plexsphere.Configuration(
    host = "http://localhost"
)

# The client must configure the authentication and authorization parameters
# in accordance with the API server security policy.
# Examples for each auth method are provided below, use the example that
# satisfies your auth use case.

# Configure Bearer authorization (JWT): operatorBearer
configuration = plexsphere.Configuration(
    access_token = os.environ["BEARER_TOKEN"]
)

# Configure API key authorization: sessionCookie
configuration.api_key['sessionCookie'] = os.environ["API_KEY"]

# Uncomment below to setup prefix (e.g. Bearer) for API key, if needed
# configuration.api_key_prefix['sessionCookie'] = 'Bearer'

# Enter a context with an instance of the API client
with plexsphere.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = plexsphere.CloudApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Project identifier (UUIDv7). Bound on `/v1/projects/{id}` for the tenancy CRUD surface, on `/v1/projects/{id}/credentials` for the operator-facing OpenBao Credential Broker inventory list, on `/v1/projects/{id}/credential-assignments` and `/v1/projects/{id}/cloud-assignments` for the assignment request/list surfaces, on `/v1/projects/{id}/blueprints` for the project-scoped Blueprint offer list, and on `/v1/projects/{id}/sink-enablements` and `/v1/projects/{id}/telemetry-routes` for the sink-enablement and Telemetry Route surfaces. 
    cursor = 'cursor_example' # str | Opaque continuation token returned by a previous call's `next_cursor`. The encoding is HMAC-signed by the server so a tampered cursor surfaces as `400`.  (optional)
    limit = 50 # int | Maximum number of items to return in a single page. A value outside [1, 200] is rejected with a `400` Problem rather than silently clamped.  (optional) (default to 50)

    try:
        # List the Cloud Assignments owned by a Project.
        api_response = api_instance.list_cloud_assignments(id, cursor=cursor, limit=limit)
        print("The response of CloudApi->list_cloud_assignments:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling CloudApi->list_cloud_assignments: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Project identifier (UUIDv7). Bound on &#x60;/v1/projects/{id}&#x60; for the tenancy CRUD surface, on &#x60;/v1/projects/{id}/credentials&#x60; for the operator-facing OpenBao Credential Broker inventory list, on &#x60;/v1/projects/{id}/credential-assignments&#x60; and &#x60;/v1/projects/{id}/cloud-assignments&#x60; for the assignment request/list surfaces, on &#x60;/v1/projects/{id}/blueprints&#x60; for the project-scoped Blueprint offer list, and on &#x60;/v1/projects/{id}/sink-enablements&#x60; and &#x60;/v1/projects/{id}/telemetry-routes&#x60; for the sink-enablement and Telemetry Route surfaces.  | 
 **cursor** | **str**| Opaque continuation token returned by a previous call&#39;s &#x60;next_cursor&#x60;. The encoding is HMAC-signed by the server so a tampered cursor surfaces as &#x60;400&#x60;.  | [optional] 
 **limit** | **int**| Maximum number of items to return in a single page. A value outside [1, 200] is rejected with a &#x60;400&#x60; Problem rather than silently clamped.  | [optional] [default to 50]

### Return type

[**CloudAssignmentList**](CloudAssignmentList.md)

### Authorization

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Page of Cloud Assignments. |  -  |
**400** | Invalid query parameters — typically a tampered or malformed cursor, an out-of-range &#x60;limit&#x60;, or a malformed Project id.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to read the parent Project (body is a &#x60;PermissionDenied&#x60; problem) OR the pagination cursor was minted by a different caller and the per-(caller, pepper) HMAC binding rejected the replay (body is a &#x60;Problem&#x60; with &#x60;code &#x3D; cursor_binding_mismatch&#x60;).  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **list_cloud_credential_clouds**
> CloudCredentialCloudList list_cloud_credential_clouds(id, cursor=cursor, limit=limit)

List the Clouds a Cloud Credential serves.

Returns a cloud_id-ordered page of the Clouds the Cloud
Credential identified by `{id}` serves — its home Cloud plus
every additional usage Cloud attached over the association API.
The handler resolves the credential's home Cloud, runs an
`observe` ReBAC check on it BEFORE the persistence read, then
pages the usage join.

The pagination cursor is HMAC-signed and bound to the
per-(caller, pepper) pseudonym, so a cursor minted by one
principal cannot be replayed by another — the cross-caller
replay surfaces as `403 cursor_binding_mismatch`. A tampered
envelope or unknown version byte stays on `400 invalid_cursor`.


### Example

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

```python
import plexsphere
from plexsphere.models.cloud_credential_cloud_list import CloudCredentialCloudList
from plexsphere.rest import ApiException
from pprint import pprint

# Defining the host is optional and defaults to http://localhost
# See configuration.py for a list of all supported configuration parameters.
configuration = plexsphere.Configuration(
    host = "http://localhost"
)

# The client must configure the authentication and authorization parameters
# in accordance with the API server security policy.
# Examples for each auth method are provided below, use the example that
# satisfies your auth use case.

# Configure Bearer authorization (JWT): operatorBearer
configuration = plexsphere.Configuration(
    access_token = os.environ["BEARER_TOKEN"]
)

# Configure API key authorization: sessionCookie
configuration.api_key['sessionCookie'] = os.environ["API_KEY"]

# Uncomment below to setup prefix (e.g. Bearer) for API key, if needed
# configuration.api_key_prefix['sessionCookie'] = 'Bearer'

# Enter a context with an instance of the API client
with plexsphere.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = plexsphere.CloudApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Cloud Credential identifier (UUIDv7). Bound on `/v1/cloud-credentials/{id}`, `/v1/cloud-credentials/{id}/revoke`, `/v1/cloud-credentials/{id}/clouds`, `/v1/cloud-credentials/{id}/credential-assignments`, and `/v1/cloud-credentials/{id}/clouds/{cloud_id}` for the operator-facing Cloud Credentials read, revoke, and usage-Cloud attach/detach surface. 
    cursor = 'cursor_example' # str | Opaque continuation token returned by a previous call's `next_cursor`. The encoding is HMAC-signed by the server so a tampered cursor surfaces as `400`.  (optional)
    limit = 50 # int | Maximum number of items to return in a single page. A value outside [1, 200] is rejected with a `400` Problem rather than silently clamped.  (optional) (default to 50)

    try:
        # List the Clouds a Cloud Credential serves.
        api_response = api_instance.list_cloud_credential_clouds(id, cursor=cursor, limit=limit)
        print("The response of CloudApi->list_cloud_credential_clouds:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling CloudApi->list_cloud_credential_clouds: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Cloud Credential identifier (UUIDv7). Bound on &#x60;/v1/cloud-credentials/{id}&#x60;, &#x60;/v1/cloud-credentials/{id}/revoke&#x60;, &#x60;/v1/cloud-credentials/{id}/clouds&#x60;, &#x60;/v1/cloud-credentials/{id}/credential-assignments&#x60;, and &#x60;/v1/cloud-credentials/{id}/clouds/{cloud_id}&#x60; for the operator-facing Cloud Credentials read, revoke, and usage-Cloud attach/detach surface.  | 
 **cursor** | **str**| Opaque continuation token returned by a previous call&#39;s &#x60;next_cursor&#x60;. The encoding is HMAC-signed by the server so a tampered cursor surfaces as &#x60;400&#x60;.  | [optional] 
 **limit** | **int**| Maximum number of items to return in a single page. A value outside [1, 200] is rejected with a &#x60;400&#x60; Problem rather than silently clamped.  | [optional] [default to 50]

### Return type

[**CloudCredentialCloudList**](CloudCredentialCloudList.md)

### Authorization

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Page of Clouds the credential serves. |  -  |
**400** | Invalid query parameters — typically a tampered or malformed cursor, an out-of-range &#x60;limit&#x60;, or a malformed Cloud Credential id.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to observe the credential&#39;s home Cloud (body is a &#x60;PermissionDenied&#x60; problem) OR the pagination cursor was minted by a different caller and the per-(caller, pepper) HMAC binding rejected the replay (body is a &#x60;Problem&#x60; with &#x60;code &#x3D; cursor_binding_mismatch&#x60;).  |  -  |
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

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

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

# The client must configure the authentication and authorization parameters
# in accordance with the API server security policy.
# Examples for each auth method are provided below, use the example that
# satisfies your auth use case.

# Configure Bearer authorization (JWT): operatorBearer
configuration = plexsphere.Configuration(
    access_token = os.environ["BEARER_TOKEN"]
)

# Configure API key authorization: sessionCookie
configuration.api_key['sessionCookie'] = os.environ["API_KEY"]

# Uncomment below to setup prefix (e.g. Bearer) for API key, if needed
# configuration.api_key_prefix['sessionCookie'] = 'Bearer'

# Enter a context with an instance of the API client
with plexsphere.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = plexsphere.CloudApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Cloud identifier (UUIDv7). Bound on `/v1/clouds/{id}` for the Cloud Inventory CRUD surface. 
    cursor = 'cursor_example' # str | Opaque continuation token returned by a previous call's `next_cursor`. The encoding is HMAC-signed by the server so a tampered cursor surfaces as `400`.  (optional)
    limit = 50 # int | Maximum number of items to return in a single page. A value outside [1, 200] is rejected with a `400` Problem rather than silently clamped.  (optional) (default to 50)

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
 **limit** | **int**| Maximum number of items to return in a single page. A value outside [1, 200] is rejected with a &#x60;400&#x60; Problem rather than silently clamped.  | [optional] [default to 50]

### Return type

[**CloudCredentialList**](CloudCredentialList.md)

### Authorization

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

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

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

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

# The client must configure the authentication and authorization parameters
# in accordance with the API server security policy.
# Examples for each auth method are provided below, use the example that
# satisfies your auth use case.

# Configure Bearer authorization (JWT): operatorBearer
configuration = plexsphere.Configuration(
    access_token = os.environ["BEARER_TOKEN"]
)

# Configure API key authorization: sessionCookie
configuration.api_key['sessionCookie'] = os.environ["API_KEY"]

# Uncomment below to setup prefix (e.g. Bearer) for API key, if needed
# configuration.api_key_prefix['sessionCookie'] = 'Bearer'

# Enter a context with an instance of the API client
with plexsphere.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = plexsphere.CloudApi(api_client)
    cursor = 'cursor_example' # str | Opaque continuation token returned by a previous call's `next_cursor`. The encoding is HMAC-signed by the server so a tampered cursor surfaces as `400`.  (optional)
    limit = 50 # int | Maximum number of items to return in a single page. A value outside [1, 200] is rejected with a `400` Problem rather than silently clamped.  (optional) (default to 50)

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
 **limit** | **int**| Maximum number of items to return in a single page. A value outside [1, 200] is rejected with a &#x60;400&#x60; Problem rather than silently clamped.  | [optional] [default to 50]

### Return type

[**CloudList**](CloudList.md)

### Authorization

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

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
handler runs a top-level `read` ReBAC check on the parent
Project BEFORE the persistence read; every assignment in the
page belongs to the one path Project, so the project `read`
check authorises the whole page and no per-row filter runs.

The projection carries the assignment identity, the owning
Project, the bound Cloud Credential, the lifecycle state, a
derived `materialised` flag, and the lifecycle timestamps.

The pagination cursor is HMAC-signed and bound to the
per-(caller, pepper) pseudonym, so a cursor minted by one
principal cannot be replayed by another — the cross-caller
replay surfaces as `403 cursor_binding_mismatch`. A tampered
envelope or unknown version byte stays on `400 invalid_cursor`.


### Example

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

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

# The client must configure the authentication and authorization parameters
# in accordance with the API server security policy.
# Examples for each auth method are provided below, use the example that
# satisfies your auth use case.

# Configure Bearer authorization (JWT): operatorBearer
configuration = plexsphere.Configuration(
    access_token = os.environ["BEARER_TOKEN"]
)

# Configure API key authorization: sessionCookie
configuration.api_key['sessionCookie'] = os.environ["API_KEY"]

# Uncomment below to setup prefix (e.g. Bearer) for API key, if needed
# configuration.api_key_prefix['sessionCookie'] = 'Bearer'

# Enter a context with an instance of the API client
with plexsphere.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = plexsphere.CloudApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Project identifier (UUIDv7). Bound on `/v1/projects/{id}` for the tenancy CRUD surface, on `/v1/projects/{id}/credentials` for the operator-facing OpenBao Credential Broker inventory list, on `/v1/projects/{id}/credential-assignments` and `/v1/projects/{id}/cloud-assignments` for the assignment request/list surfaces, on `/v1/projects/{id}/blueprints` for the project-scoped Blueprint offer list, and on `/v1/projects/{id}/sink-enablements` and `/v1/projects/{id}/telemetry-routes` for the sink-enablement and Telemetry Route surfaces. 
    cursor = 'cursor_example' # str | Opaque continuation token returned by a previous call's `next_cursor`. The encoding is HMAC-signed by the server so a tampered cursor surfaces as `400`.  (optional)
    limit = 50 # int | Maximum number of items to return in a single page. A value outside [1, 200] is rejected with a `400` Problem rather than silently clamped.  (optional) (default to 50)

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
 **id** | **UUID**| Project identifier (UUIDv7). Bound on &#x60;/v1/projects/{id}&#x60; for the tenancy CRUD surface, on &#x60;/v1/projects/{id}/credentials&#x60; for the operator-facing OpenBao Credential Broker inventory list, on &#x60;/v1/projects/{id}/credential-assignments&#x60; and &#x60;/v1/projects/{id}/cloud-assignments&#x60; for the assignment request/list surfaces, on &#x60;/v1/projects/{id}/blueprints&#x60; for the project-scoped Blueprint offer list, and on &#x60;/v1/projects/{id}/sink-enablements&#x60; and &#x60;/v1/projects/{id}/telemetry-routes&#x60; for the sink-enablement and Telemetry Route surfaces.  | 
 **cursor** | **str**| Opaque continuation token returned by a previous call&#39;s &#x60;next_cursor&#x60;. The encoding is HMAC-signed by the server so a tampered cursor surfaces as &#x60;400&#x60;.  | [optional] 
 **limit** | **int**| Maximum number of items to return in a single page. A value outside [1, 200] is rejected with a &#x60;400&#x60; Problem rather than silently clamped.  | [optional] [default to 50]

### Return type

[**CredentialAssignmentList**](CredentialAssignmentList.md)

### Authorization

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Page of Credential Assignments. |  -  |
**400** | Invalid query parameters — typically a tampered or malformed cursor, an out-of-range &#x60;limit&#x60;, or a malformed Project id.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to read the parent Project (body is a &#x60;PermissionDenied&#x60; problem) OR the pagination cursor was minted by a different caller and the per-(caller, pepper) HMAC binding rejected the replay (body is a &#x60;Problem&#x60; with &#x60;code &#x3D; cursor_binding_mismatch&#x60;).  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **list_provider_bundle_clouds**
> ProviderBundleCloudList list_provider_bundle_clouds(id, cursor=cursor, limit=limit)

List the Clouds that reference a provider bundle.

Returns a cloud_id-ordered page of the Clouds that take their
provider configuration from the provider bundle identified by
`{id}`. Each item carries the Cloud's id, slug, and display
name, so a console rendering the roster needs no follow-up read
per row.

The gate is `provider_bundle#manage` on the addressed bundle —
the permission `PatchProviderBundle` and `DeleteProviderBundle`
require, NOT the `observe` the other read verbs use — run BEFORE
the persistence read. A roster row names a Cloud in whatever
Domain holds it, and holding a permission on a bundle grants no
`cloud#observe` anywhere, so the roster is scoped to the caller
who is about to edit or delete the bundle rather than to every
caller who may read it.

Under that gate the items are deliberately NOT filtered per
Cloud: a per-row visibility filter would drop the Clouds the
caller cannot observe individually and under-report how far a
bundle edit reaches, which is the question the endpoint exists
to answer, and would contradict the unfiltered
`referencing_clouds` count a refused delete already carries.

The pagination cursor is HMAC-signed and bound to the
per-(caller, pepper) pseudonym, so a cursor minted by one
principal cannot be replayed by another — the cross-caller
replay surfaces as `403 cursor_binding_mismatch`. It is bound to
this operation as well: a cursor minted by `ListProviderBundles`
carries a different surface version byte and is refused with
`400 invalid_cursor`, and so is a roster cursor presented there.
A tampered envelope stays on `400 invalid_cursor` too.


### Example

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

```python
import plexsphere
from plexsphere.models.provider_bundle_cloud_list import ProviderBundleCloudList
from plexsphere.rest import ApiException
from pprint import pprint

# Defining the host is optional and defaults to http://localhost
# See configuration.py for a list of all supported configuration parameters.
configuration = plexsphere.Configuration(
    host = "http://localhost"
)

# The client must configure the authentication and authorization parameters
# in accordance with the API server security policy.
# Examples for each auth method are provided below, use the example that
# satisfies your auth use case.

# Configure Bearer authorization (JWT): operatorBearer
configuration = plexsphere.Configuration(
    access_token = os.environ["BEARER_TOKEN"]
)

# Configure API key authorization: sessionCookie
configuration.api_key['sessionCookie'] = os.environ["API_KEY"]

# Uncomment below to setup prefix (e.g. Bearer) for API key, if needed
# configuration.api_key_prefix['sessionCookie'] = 'Bearer'

# Enter a context with an instance of the API client
with plexsphere.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = plexsphere.CloudApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Provider bundle identifier (UUIDv7). Bound on `/v1/provider-bundles/{id}` for the Cloud Inventory provider-bundle CRUD surface, and on `/v1/provider-bundles/{id}/clouds` for the roster of Clouds that reference the bundle. 
    cursor = 'cursor_example' # str | Opaque continuation token returned by a previous call's `next_cursor`. The encoding is HMAC-signed by the server so a tampered cursor surfaces as `400`.  (optional)
    limit = 50 # int | Maximum number of items to return in a single page. A value outside [1, 200] is rejected with a `400` Problem rather than silently clamped.  (optional) (default to 50)

    try:
        # List the Clouds that reference a provider bundle.
        api_response = api_instance.list_provider_bundle_clouds(id, cursor=cursor, limit=limit)
        print("The response of CloudApi->list_provider_bundle_clouds:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling CloudApi->list_provider_bundle_clouds: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Provider bundle identifier (UUIDv7). Bound on &#x60;/v1/provider-bundles/{id}&#x60; for the Cloud Inventory provider-bundle CRUD surface, and on &#x60;/v1/provider-bundles/{id}/clouds&#x60; for the roster of Clouds that reference the bundle.  | 
 **cursor** | **str**| Opaque continuation token returned by a previous call&#39;s &#x60;next_cursor&#x60;. The encoding is HMAC-signed by the server so a tampered cursor surfaces as &#x60;400&#x60;.  | [optional] 
 **limit** | **int**| Maximum number of items to return in a single page. A value outside [1, 200] is rejected with a &#x60;400&#x60; Problem rather than silently clamped.  | [optional] [default to 50]

### Return type

[**ProviderBundleCloudList**](ProviderBundleCloudList.md)

### Authorization

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Page of Clouds referencing the provider bundle. |  -  |
**400** | Invalid request parameters — a malformed provider bundle id, a tampered or malformed cursor, or an out-of-range &#x60;limit&#x60;. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;invalid_provider_bundle_id&#x60;, &#x60;invalid_cursor&#x60;, &#x60;invalid_limit&#x60; }.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to manage the addressed provider bundle (body is a &#x60;PermissionDenied&#x60; problem) OR the pagination cursor was minted by a different caller and the per-(caller, pepper) HMAC binding rejected the replay (body is a &#x60;Problem&#x60; with &#x60;code &#x3D; cursor_binding_mismatch&#x60;).  |  -  |
**404** | Provider bundle not found. Body is a &#x60;Problem&#x60; with &#x60;code: provider_bundle_not_found&#x60;. Reached only by a caller the graph already grants &#x60;manage&#x60; on that id, for the reason &#x60;GetProviderBundle&#x60; records.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **list_provider_bundle_versions**
> ProviderBundleVersionList list_provider_bundle_versions(id, cursor=cursor, limit=limit)

List the published versions of a provider bundle.

Returns a newest-first page of the content versions the provider
bundle identified by `{id}` has published. Each item carries the
whole declaration that version froze — its package set and the
apiVersion those packages serve their ProviderConfig under — so
a client comparing two versions needs no follow-up read per row.

A published version is immutable. A content patch on the bundle
appends the next one and rewrites none of the existing rows, so
the declaration a Cloud pins answers the same bytes for as long
as the pin stands.

The gate is `provider_bundle#observe` on the addressed bundle —
the permission `GetProviderBundle` requires — run BEFORE the
persistence read, so an unauthorised caller never learns from the
response whether the bundle exists. The history is bundle
content rather than a roster of the Clouds holding it, which is
why the gate is the read one and not the `manage`
`ListProviderBundleClouds` requires.

The pagination cursor is HMAC-signed and bound to the
per-(caller, pepper) pseudonym, so a cursor minted by one
principal cannot be replayed by another — the cross-caller
replay surfaces as `403 cursor_binding_mismatch`. It is bound to
this operation as well: a cursor minted by `ListProviderBundles`
or `ListProviderBundleClouds` carries a different surface version
byte and is refused with `400 invalid_cursor`. A tampered
envelope stays on `400 invalid_cursor` too.


### Example

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

```python
import plexsphere
from plexsphere.models.provider_bundle_version_list import ProviderBundleVersionList
from plexsphere.rest import ApiException
from pprint import pprint

# Defining the host is optional and defaults to http://localhost
# See configuration.py for a list of all supported configuration parameters.
configuration = plexsphere.Configuration(
    host = "http://localhost"
)

# The client must configure the authentication and authorization parameters
# in accordance with the API server security policy.
# Examples for each auth method are provided below, use the example that
# satisfies your auth use case.

# Configure Bearer authorization (JWT): operatorBearer
configuration = plexsphere.Configuration(
    access_token = os.environ["BEARER_TOKEN"]
)

# Configure API key authorization: sessionCookie
configuration.api_key['sessionCookie'] = os.environ["API_KEY"]

# Uncomment below to setup prefix (e.g. Bearer) for API key, if needed
# configuration.api_key_prefix['sessionCookie'] = 'Bearer'

# Enter a context with an instance of the API client
with plexsphere.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = plexsphere.CloudApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Provider bundle identifier (UUIDv7). Bound on `/v1/provider-bundles/{id}` for the Cloud Inventory provider-bundle CRUD surface, and on `/v1/provider-bundles/{id}/clouds` for the roster of Clouds that reference the bundle. 
    cursor = 'cursor_example' # str | Opaque continuation token returned by a previous call's `next_cursor`. The encoding is HMAC-signed by the server so a tampered cursor surfaces as `400`.  (optional)
    limit = 50 # int | Maximum number of items to return in a single page. A value outside [1, 200] is rejected with a `400` Problem rather than silently clamped.  (optional) (default to 50)

    try:
        # List the published versions of a provider bundle.
        api_response = api_instance.list_provider_bundle_versions(id, cursor=cursor, limit=limit)
        print("The response of CloudApi->list_provider_bundle_versions:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling CloudApi->list_provider_bundle_versions: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Provider bundle identifier (UUIDv7). Bound on &#x60;/v1/provider-bundles/{id}&#x60; for the Cloud Inventory provider-bundle CRUD surface, and on &#x60;/v1/provider-bundles/{id}/clouds&#x60; for the roster of Clouds that reference the bundle.  | 
 **cursor** | **str**| Opaque continuation token returned by a previous call&#39;s &#x60;next_cursor&#x60;. The encoding is HMAC-signed by the server so a tampered cursor surfaces as &#x60;400&#x60;.  | [optional] 
 **limit** | **int**| Maximum number of items to return in a single page. A value outside [1, 200] is rejected with a &#x60;400&#x60; Problem rather than silently clamped.  | [optional] [default to 50]

### Return type

[**ProviderBundleVersionList**](ProviderBundleVersionList.md)

### Authorization

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Page of versions the provider bundle has published. |  -  |
**400** | Invalid request parameters — a malformed provider bundle id, a tampered or malformed cursor, or an out-of-range &#x60;limit&#x60;. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;invalid_provider_bundle_id&#x60;, &#x60;invalid_cursor&#x60;, &#x60;invalid_limit&#x60; }.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to observe the addressed provider bundle (body is a &#x60;PermissionDenied&#x60; problem) OR the pagination cursor was minted by a different caller and the per-(caller, pepper) HMAC binding rejected the replay (body is a &#x60;Problem&#x60; with &#x60;code &#x3D; cursor_binding_mismatch&#x60;).  |  -  |
**404** | Provider bundle not found. Body is a &#x60;Problem&#x60; with &#x60;code: provider_bundle_not_found&#x60;. Reached only by a caller the graph already grants &#x60;observe&#x60; on that id, for the reason &#x60;GetProviderBundle&#x60; records.  |  -  |
**500** | Internal server error. |  -  |
**503** | The authorization backend is temporarily unreachable, so the &#x60;observe&#x60; gate could not be decided. Body is a &#x60;Problem&#x60; with &#x60;code: authz_unavailable&#x60;. The call is retryable — the code is distinct from the &#x60;403&#x60; a real denial answers.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **list_provider_bundles**
> ProviderBundleList list_provider_bundles(cursor=cursor, limit=limit)

List provider bundles.

Returns a slug-ordered page of `ProviderBundle` aggregates the
caller is authorised to see. Per-row visibility is layered on
top of the page: rows the caller cannot `observe` are filtered
out so the response items are a subset of the persistence-level
page.

The pagination cursor is HMAC-signed and bound to the
per-(caller, pepper) pseudonym, so a cursor minted by one
principal cannot be replayed by another — the cross-caller
replay surfaces as `403 cursor_binding_mismatch`. A tampered
envelope or unknown version byte stays on `400 invalid_cursor`.


### Example

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

```python
import plexsphere
from plexsphere.models.provider_bundle_list import ProviderBundleList
from plexsphere.rest import ApiException
from pprint import pprint

# Defining the host is optional and defaults to http://localhost
# See configuration.py for a list of all supported configuration parameters.
configuration = plexsphere.Configuration(
    host = "http://localhost"
)

# The client must configure the authentication and authorization parameters
# in accordance with the API server security policy.
# Examples for each auth method are provided below, use the example that
# satisfies your auth use case.

# Configure Bearer authorization (JWT): operatorBearer
configuration = plexsphere.Configuration(
    access_token = os.environ["BEARER_TOKEN"]
)

# Configure API key authorization: sessionCookie
configuration.api_key['sessionCookie'] = os.environ["API_KEY"]

# Uncomment below to setup prefix (e.g. Bearer) for API key, if needed
# configuration.api_key_prefix['sessionCookie'] = 'Bearer'

# Enter a context with an instance of the API client
with plexsphere.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = plexsphere.CloudApi(api_client)
    cursor = 'cursor_example' # str | Opaque continuation token returned by a previous call's `next_cursor`. The encoding is HMAC-signed by the server so a tampered cursor surfaces as `400`.  (optional)
    limit = 50 # int | Maximum number of items to return in a single page. A value outside [1, 200] is rejected with a `400` Problem rather than silently clamped.  (optional) (default to 50)

    try:
        # List provider bundles.
        api_response = api_instance.list_provider_bundles(cursor=cursor, limit=limit)
        print("The response of CloudApi->list_provider_bundles:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling CloudApi->list_provider_bundles: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **cursor** | **str**| Opaque continuation token returned by a previous call&#39;s &#x60;next_cursor&#x60;. The encoding is HMAC-signed by the server so a tampered cursor surfaces as &#x60;400&#x60;.  | [optional] 
 **limit** | **int**| Maximum number of items to return in a single page. A value outside [1, 200] is rejected with a &#x60;400&#x60; Problem rather than silently clamped.  | [optional] [default to 50]

### Return type

[**ProviderBundleList**](ProviderBundleList.md)

### Authorization

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Page of provider bundles. |  -  |
**400** | Invalid query parameters — typically a tampered or malformed cursor or an out-of-range &#x60;limit&#x60;. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;invalid_cursor&#x60;, &#x60;invalid_limit&#x60; }.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to list provider bundles (body is a &#x60;PermissionDenied&#x60; problem) OR the pagination cursor was minted by a different caller and the per-(caller, pepper) HMAC binding rejected the replay (body is a &#x60;Problem&#x60; with &#x60;code &#x3D; cursor_binding_mismatch&#x60;).  |  -  |
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

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

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

# The client must configure the authentication and authorization parameters
# in accordance with the API server security policy.
# Examples for each auth method are provided below, use the example that
# satisfies your auth use case.

# Configure Bearer authorization (JWT): operatorBearer
configuration = plexsphere.Configuration(
    access_token = os.environ["BEARER_TOKEN"]
)

# Configure API key authorization: sessionCookie
configuration.api_key['sessionCookie'] = os.environ["API_KEY"]

# Uncomment below to setup prefix (e.g. Bearer) for API key, if needed
# configuration.api_key_prefix['sessionCookie'] = 'Bearer'

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

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Cloud patched. |  -  |
**400** | Invalid body. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;invalid_cloud&#x60;, &#x60;invalid_cloud_endpoint&#x60;, &#x60;invalid_cloud_region_defaults&#x60;, &#x60;slug_immutable&#x60;, &#x60;provider_immutable&#x60;, &#x60;empty_patch&#x60;, &#x60;invalid_cloud_provider_mode&#x60;, &#x60;unknown_provider_bundle&#x60;, &#x60;provider_bundle_provider_mismatch&#x60;, &#x60;invalid_body&#x60; }.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to manage the addressed Cloud, or — when the body touches the bundle reference — holds NEITHER &#x60;platform#manage&#x60; NOR &#x60;observe&#x60; on the provider bundle involved. &#x60;cloud#manage&#x60; is a grant on the Cloud alone and confers nothing on the platform-scoped bundle catalogue, so the reference is authorized separately; &#x60;platform#manage&#x60; — the permission &#x60;CreateCloud&#x60; requires to make the same reference — satisfies it on its own, so a bundle created moments earlier is referenceable before its authorization tuples have replicated.  A body naming a well-formed &#x60;provider_bundle_id&#x60; is checked against that bundle. A promotion carries no id, so the bundle checked is the one the addressed Cloud already references: each &#x60;200&#x60; renders the frozen declaration of one historical version, which &#x60;GET /v1/provider-bundles/{id}/versions&#x60; refuses without &#x60;observe&#x60;, and the pin moves in both directions. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Cloud not found. Body is a &#x60;Problem&#x60; with &#x60;code: cloud_not_found&#x60;.  |  -  |
**409** | The Cloud&#39;s bundle reference moved under the request. Body is a &#x60;Problem&#x60; with &#x60;code: provider_bundle_reference_moved&#x60;.  Two patch shapes carry the precondition, both of them naming no &#x60;provider_bundle_id&#x60; while depending on the one the Cloud holds. A promotion is authorized against the bundle the addressed Cloud referenced when the request was read; a &#x60;provider_package_overrides&#x60; patch states versions chosen against the declaration the Cloud pinned at that moment. The reference is asserted as the pair it is — the bundle AND the version pinned on it — because the write rewrites both. A competing writer that re-points the Cloud at a different bundle, or promotes it to another version, before the write would otherwise move the pin inside a bundle nobody authorized, or revert the competing write while reporting success. Nothing was written; re-read the Cloud and send the patch again.  The reference asserted is the one this request read, so the window covered runs from that read to the write. A writer that moved the reference between your own &#x60;GET&#x60; and this request is not reported: the patch is applied to the Cloud as it stands.  |  -  |
**413** | Request body exceeded the 8 KiB Cloud Inventory ceiling.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **patch_provider_bundle**
> ProviderBundleResponse patch_provider_bundle(id, provider_bundle_patch_request)

Patch mutable fields on a provider bundle.

Patches the `ProviderBundle` identified by `{id}`. The body
MUST set at least one of `display_name`, `provider_packages`,
or `provider_config_api_version` — an empty body surfaces as
`400 empty_patch`. A `provider_packages` patch replaces the
whole set: the packages the body names become the bundle's
packages and every package it omits is dropped.

A content patch — one carrying `provider_packages` or
`provider_config_api_version` — publishes a NEW immutable
version of the bundle rather than rewriting the current one.
The two content fields publish one version between them even
when the body names both, and the response reports the number
that version was published under in `latest_version`. A
rename-only patch publishes nothing.

Publishing moves no Cloud. Every Cloud referencing the bundle
keeps serving the version it pins and converges with that
declaration until a Cloud write moves the pin — a
`PATCH /v1/clouds/{id}` naming `provider_bundle_version`, which
promotes that one Cloud. Read the published history at
`GET /v1/provider-bundles/{id}/versions` to see which versions
a Cloud can be promoted onto.

DECISION: BOTH `slug` and `provider` are intentionally NOT
patchable fields. The slug is the URL handle an operator types
to address the bundle, so re-slugging in place breaks every
bookmarked URL. The provider is the compatibility key a bundle
is checked against before a Cloud may reference it: every
stored reference was admitted against the provider the bundle
carried at the time, and nothing re-checks a reference once it
is stored. The handler rejects any body that carries a `slug`
key (even with the same value) with `400 slug_immutable`, and
any body that carries a `provider` key with
`400 provider_immutable`.

The read and the write are two transactions, so the merged
aggregate can be stale by the time it is written. The write is
gated on the version the read observed and a competing writer
that got there first surfaces as `409 provider_bundle_stale`;
nothing is written and no event is emitted in that case.


### Example

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

```python
import plexsphere
from plexsphere.models.provider_bundle_patch_request import ProviderBundlePatchRequest
from plexsphere.models.provider_bundle_response import ProviderBundleResponse
from plexsphere.rest import ApiException
from pprint import pprint

# Defining the host is optional and defaults to http://localhost
# See configuration.py for a list of all supported configuration parameters.
configuration = plexsphere.Configuration(
    host = "http://localhost"
)

# The client must configure the authentication and authorization parameters
# in accordance with the API server security policy.
# Examples for each auth method are provided below, use the example that
# satisfies your auth use case.

# Configure Bearer authorization (JWT): operatorBearer
configuration = plexsphere.Configuration(
    access_token = os.environ["BEARER_TOKEN"]
)

# Configure API key authorization: sessionCookie
configuration.api_key['sessionCookie'] = os.environ["API_KEY"]

# Uncomment below to setup prefix (e.g. Bearer) for API key, if needed
# configuration.api_key_prefix['sessionCookie'] = 'Bearer'

# Enter a context with an instance of the API client
with plexsphere.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = plexsphere.CloudApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Provider bundle identifier (UUIDv7). Bound on `/v1/provider-bundles/{id}` for the Cloud Inventory provider-bundle CRUD surface, and on `/v1/provider-bundles/{id}/clouds` for the roster of Clouds that reference the bundle. 
    provider_bundle_patch_request = {"display_name":"AWS baseline packages (2026 Q2)","provider_packages":[{"source":"xpkg.upbound.io/upbound/provider-aws-ec2","version":"v2.7.0"},{"source":"xpkg.upbound.io/upbound/provider-aws-s3","version":"v2.7.0"}]} # ProviderBundlePatchRequest | 

    try:
        # Patch mutable fields on a provider bundle.
        api_response = api_instance.patch_provider_bundle(id, provider_bundle_patch_request)
        print("The response of CloudApi->patch_provider_bundle:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling CloudApi->patch_provider_bundle: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Provider bundle identifier (UUIDv7). Bound on &#x60;/v1/provider-bundles/{id}&#x60; for the Cloud Inventory provider-bundle CRUD surface, and on &#x60;/v1/provider-bundles/{id}/clouds&#x60; for the roster of Clouds that reference the bundle.  | 
 **provider_bundle_patch_request** | [**ProviderBundlePatchRequest**](ProviderBundlePatchRequest.md)|  | 

### Return type

[**ProviderBundleResponse**](ProviderBundleResponse.md)

### Authorization

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Provider bundle patched. |  -  |
**400** | Invalid body. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;invalid_provider_bundle&#x60;, &#x60;invalid_provider_bundle_id&#x60;, &#x60;slug_immutable&#x60;, &#x60;provider_immutable&#x60;, &#x60;empty_patch&#x60;, &#x60;invalid_body&#x60; }.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to manage the addressed provider bundle. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Provider bundle not found. Body is a &#x60;Problem&#x60; with &#x60;code: provider_bundle_not_found&#x60;.  |  -  |
**409** | Conflict — a competing writer overtook the read the patch was derived from, or the merged state collides with a persisted row. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;provider_bundle_stale&#x60;, &#x60;provider_bundle_slug_conflict&#x60;, &#x60;provider_bundle_conflict&#x60; }.  |  -  |
**413** | Request body exceeded the 8 KiB Cloud Inventory ceiling. Body is a &#x60;Problem&#x60; with &#x60;code: request_body_too_large&#x60;.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **request_cloud_assignment**
> CloudAssignmentResponse request_cloud_assignment(id, cloud_assignment_request_body)

Request usage of a Cloud for a Project.

Opens a Cloud Assignment request that asks for the Cloud named
in the body to be made usable in the Project identified by
`{id}`. The handler runs a `deploy` ReBAC check on the parent
Project BEFORE the persistence write, then delegates to the
Cloud Assignment application service which records the request in
the `requested` state and appends a `CloudAssignmentRequested`
outbox event in a single transaction.

The request is not yet usable — the Cloud only becomes usable in
the Project once an operator approves it. A second open request
for the same (Project, Cloud) pair while an earlier one is still
live is rejected with `409 duplicate_live_cloud_assignment`.


### Example

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

```python
import plexsphere
from plexsphere.models.cloud_assignment_request_body import CloudAssignmentRequestBody
from plexsphere.models.cloud_assignment_response import CloudAssignmentResponse
from plexsphere.rest import ApiException
from pprint import pprint

# Defining the host is optional and defaults to http://localhost
# See configuration.py for a list of all supported configuration parameters.
configuration = plexsphere.Configuration(
    host = "http://localhost"
)

# The client must configure the authentication and authorization parameters
# in accordance with the API server security policy.
# Examples for each auth method are provided below, use the example that
# satisfies your auth use case.

# Configure Bearer authorization (JWT): operatorBearer
configuration = plexsphere.Configuration(
    access_token = os.environ["BEARER_TOKEN"]
)

# Configure API key authorization: sessionCookie
configuration.api_key['sessionCookie'] = os.environ["API_KEY"]

# Uncomment below to setup prefix (e.g. Bearer) for API key, if needed
# configuration.api_key_prefix['sessionCookie'] = 'Bearer'

# Enter a context with an instance of the API client
with plexsphere.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = plexsphere.CloudApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Project identifier (UUIDv7). Bound on `/v1/projects/{id}` for the tenancy CRUD surface, on `/v1/projects/{id}/credentials` for the operator-facing OpenBao Credential Broker inventory list, on `/v1/projects/{id}/credential-assignments` and `/v1/projects/{id}/cloud-assignments` for the assignment request/list surfaces, on `/v1/projects/{id}/blueprints` for the project-scoped Blueprint offer list, and on `/v1/projects/{id}/sink-enablements` and `/v1/projects/{id}/telemetry-routes` for the sink-enablement and Telemetry Route surfaces. 
    cloud_assignment_request_body = {"cloud_id":"0190a8b8-a0c0-7a0a-8a0a-a0a0a0a0a0c1"} # CloudAssignmentRequestBody | 

    try:
        # Request usage of a Cloud for a Project.
        api_response = api_instance.request_cloud_assignment(id, cloud_assignment_request_body)
        print("The response of CloudApi->request_cloud_assignment:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling CloudApi->request_cloud_assignment: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Project identifier (UUIDv7). Bound on &#x60;/v1/projects/{id}&#x60; for the tenancy CRUD surface, on &#x60;/v1/projects/{id}/credentials&#x60; for the operator-facing OpenBao Credential Broker inventory list, on &#x60;/v1/projects/{id}/credential-assignments&#x60; and &#x60;/v1/projects/{id}/cloud-assignments&#x60; for the assignment request/list surfaces, on &#x60;/v1/projects/{id}/blueprints&#x60; for the project-scoped Blueprint offer list, and on &#x60;/v1/projects/{id}/sink-enablements&#x60; and &#x60;/v1/projects/{id}/telemetry-routes&#x60; for the sink-enablement and Telemetry Route surfaces.  | 
 **cloud_assignment_request_body** | [**CloudAssignmentRequestBody**](CloudAssignmentRequestBody.md)|  | 

### Return type

[**CloudAssignmentResponse**](CloudAssignmentResponse.md)

### Authorization

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**201** | Cloud Assignment opened in the &#x60;requested&#x60; state. No &#x60;Location&#x60; header is sent — the aggregate has no by-id read surface; poll it through the Project&#39;s assignment list.  |  -  |
**400** | Malformed id or body. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;invalid_project_id&#x60;, &#x60;invalid_body&#x60;, &#x60;invalid_cloud_id&#x60; }.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to deploy in the parent Project. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**409** | A live Cloud Assignment already exists for the same (Project, Cloud) pair. Body is a &#x60;Problem&#x60; with &#x60;code: duplicate_live_cloud_assignment&#x60;.  |  -  |
**413** | Request body exceeded the 8 KiB ceiling. Body is a &#x60;Problem&#x60; with &#x60;code: request_body_too_large&#x60;.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **request_credential_assignment**
> CredentialAssignmentResponse request_credential_assignment(id, credential_assignment_request)

Request a Credential Assignment for a Project.

Opens a Credential Assignment request that binds a Cloud
Credential to the Project identified by `{id}`. The body names
the credential either directly (`cloud_credential_id`) or
indirectly by Cloud (`cloud_id`), in which case the system
auto-selects the most recently issued eligible credential
serving that Cloud. The handler runs a `deploy` ReBAC check on
the parent Project BEFORE the persistence write, then delegates
to the Credential Assignment application service which records
the request in the `requested` state and appends a
`CredentialAssignmentRequested` outbox event in a single
transaction.

The newly opened assignment is not yet materialised — the
binding only becomes live once an approver moves it to the
`approved` state. A second open request for the same
(Project, Cloud Credential) pair while an earlier one is still
live is rejected with `409 duplicate_live_assignment`. A Cloud
Credential that is not in an assignable lifecycle state is
rejected with `422 credential_not_assignable`.

Either form requires the Cloud the credential belongs to to be
usable in the Project — an approved Cloud Assignment — otherwise
the request is rejected with `422 cloud_not_usable_in_project`. A
credential is only usable where both assignments are in place, so
a request opened without the Cloud Assignment would be approved
and still refused at deploy time. The `cloud_id` form checks the
Cloud it names before auto-selecting, and both forms then check
the Cloud the selected credential actually belongs to — which the
credential-to-Cloud usage join may make a different one.

For the `cloud_id` form additionally: at least one eligible
credential must serve the Cloud — otherwise
`422 no_eligible_credential_for_cloud`.


### Example

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

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

# The client must configure the authentication and authorization parameters
# in accordance with the API server security policy.
# Examples for each auth method are provided below, use the example that
# satisfies your auth use case.

# Configure Bearer authorization (JWT): operatorBearer
configuration = plexsphere.Configuration(
    access_token = os.environ["BEARER_TOKEN"]
)

# Configure API key authorization: sessionCookie
configuration.api_key['sessionCookie'] = os.environ["API_KEY"]

# Uncomment below to setup prefix (e.g. Bearer) for API key, if needed
# configuration.api_key_prefix['sessionCookie'] = 'Bearer'

# Enter a context with an instance of the API client
with plexsphere.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = plexsphere.CloudApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Project identifier (UUIDv7). Bound on `/v1/projects/{id}` for the tenancy CRUD surface, on `/v1/projects/{id}/credentials` for the operator-facing OpenBao Credential Broker inventory list, on `/v1/projects/{id}/credential-assignments` and `/v1/projects/{id}/cloud-assignments` for the assignment request/list surfaces, on `/v1/projects/{id}/blueprints` for the project-scoped Blueprint offer list, and on `/v1/projects/{id}/sink-enablements` and `/v1/projects/{id}/telemetry-routes` for the sink-enablement and Telemetry Route surfaces. 
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
 **id** | **UUID**| Project identifier (UUIDv7). Bound on &#x60;/v1/projects/{id}&#x60; for the tenancy CRUD surface, on &#x60;/v1/projects/{id}/credentials&#x60; for the operator-facing OpenBao Credential Broker inventory list, on &#x60;/v1/projects/{id}/credential-assignments&#x60; and &#x60;/v1/projects/{id}/cloud-assignments&#x60; for the assignment request/list surfaces, on &#x60;/v1/projects/{id}/blueprints&#x60; for the project-scoped Blueprint offer list, and on &#x60;/v1/projects/{id}/sink-enablements&#x60; and &#x60;/v1/projects/{id}/telemetry-routes&#x60; for the sink-enablement and Telemetry Route surfaces.  | 
 **credential_assignment_request** | [**CredentialAssignmentRequest**](CredentialAssignmentRequest.md)|  | 

### Return type

[**CredentialAssignmentResponse**](CredentialAssignmentResponse.md)

### Authorization

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**201** | Credential Assignment opened in the &#x60;requested&#x60; state. No &#x60;Location&#x60; header is sent — the aggregate has no by-id read surface; poll it through the Project&#39;s assignment list.  |  -  |
**400** | Malformed id or body. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;invalid_project_id&#x60;, &#x60;invalid_body&#x60;, &#x60;invalid_cloud_credential_id&#x60;, &#x60;invalid_cloud_id&#x60;, &#x60;ambiguous_credential_target&#x60; }. &#x60;ambiguous_credential_target&#x60; is returned when both &#x60;cloud_credential_id&#x60; and &#x60;cloud_id&#x60; are supplied; &#x60;invalid_body&#x60; when neither is.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to deploy in the parent Project. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**409** | A live Credential Assignment already exists for the same (Project, Cloud Credential) pair. Body is a &#x60;Problem&#x60; with &#x60;code: duplicate_live_assignment&#x60;.  |  -  |
**422** | Semantic rejection. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;credential_not_assignable&#x60;, &#x60;cloud_not_usable_in_project&#x60;, &#x60;no_eligible_credential_for_cloud&#x60; }. &#x60;credential_not_assignable&#x60; — the named Cloud Credential is not in an assignable lifecycle state (a &#x60;cloud_credential_id&#x60; naming no credential at all surfaces through the same code — a missing reference is unusable for the same reason a revoked one is). &#x60;cloud_not_usable_in_project&#x60; — a Cloud with no approved assignment to the Project: either the Cloud the &#x60;cloud_id&#x60; form named, or the Cloud the requested credential belongs to. &#x60;no_eligible_credential_for_cloud&#x60; — the &#x60;cloud_id&#x60; form named a usable Cloud that has no eligible credential.  |  -  |
**413** | Request body exceeded the 8 KiB ceiling. Body is a &#x60;Problem&#x60; with &#x60;code: request_body_too_large&#x60;.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **revoke_cloud_assignment**
> CloudAssignmentResponse revoke_cloud_assignment(id, cloud_assignment_decision_request)

Revoke a Cloud Assignment.

Revokes the Cloud Assignment identified by `{id}`. The handler
reads the row to resolve the owning Cloud and the consuming
Project, runs the dual ReBAC gate described below, then
delegates to the Cloud Assignment application service which
moves the assignment to the `revoked` state, narrow-deletes the
`cloud#uses` binding so the Cloud is no longer usable in the
Project, and appends a `CloudAssignmentRevoked` outbox event in
a single transaction. The `reason` from the body is recorded on
the event as an operator-supplied audit string.

The gate has two legs: `assign` on the owning Cloud, the object
the assignment spends, and on its denial `deploy` on the
consuming Project. Either leg authorises the revocation; the
call is denied only when both deny. The second leg lets a
Project owner hand back a Cloud they were granted without
holding the Cloud-side `assign` relation.

Revocation is only legal from the `approved` state — any other
source state returns `409 illegal_transition`.


### Example

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

```python
import plexsphere
from plexsphere.models.cloud_assignment_decision_request import CloudAssignmentDecisionRequest
from plexsphere.models.cloud_assignment_response import CloudAssignmentResponse
from plexsphere.rest import ApiException
from pprint import pprint

# Defining the host is optional and defaults to http://localhost
# See configuration.py for a list of all supported configuration parameters.
configuration = plexsphere.Configuration(
    host = "http://localhost"
)

# The client must configure the authentication and authorization parameters
# in accordance with the API server security policy.
# Examples for each auth method are provided below, use the example that
# satisfies your auth use case.

# Configure Bearer authorization (JWT): operatorBearer
configuration = plexsphere.Configuration(
    access_token = os.environ["BEARER_TOKEN"]
)

# Configure API key authorization: sessionCookie
configuration.api_key['sessionCookie'] = os.environ["API_KEY"]

# Uncomment below to setup prefix (e.g. Bearer) for API key, if needed
# configuration.api_key_prefix['sessionCookie'] = 'Bearer'

# Enter a context with an instance of the API client
with plexsphere.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = plexsphere.CloudApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Cloud Assignment identifier (UUIDv7). Bound on `/v1/cloud-assignments/{id}/revoke` for the Cloud Assignment revocation surface. 
    cloud_assignment_decision_request = {"reason":"Project no longer authorised for this Cloud"} # CloudAssignmentDecisionRequest | 

    try:
        # Revoke a Cloud Assignment.
        api_response = api_instance.revoke_cloud_assignment(id, cloud_assignment_decision_request)
        print("The response of CloudApi->revoke_cloud_assignment:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling CloudApi->revoke_cloud_assignment: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Cloud Assignment identifier (UUIDv7). Bound on &#x60;/v1/cloud-assignments/{id}/revoke&#x60; for the Cloud Assignment revocation surface.  | 
 **cloud_assignment_decision_request** | [**CloudAssignmentDecisionRequest**](CloudAssignmentDecisionRequest.md)|  | 

### Return type

[**CloudAssignmentResponse**](CloudAssignmentResponse.md)

### Authorization

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Cloud Assignment revoked. Body is the metadata projection with &#x60;state: revoked&#x60;.  |  -  |
**400** | Malformed id or body. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;invalid_cloud_assignment_id&#x60;, &#x60;invalid_body&#x60;, &#x60;invalid_decision_reason&#x60; }.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller holds neither &#x60;assign&#x60; on the owning Cloud nor &#x60;deploy&#x60; on the consuming Project. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Cloud Assignment not found. Body is a &#x60;Problem&#x60; with &#x60;code: cloud_assignment_not_found&#x60;.  |  -  |
**409** | The assignment is not in a state from which revocation is legal. Body is a &#x60;Problem&#x60; with &#x60;code: illegal_transition&#x60;.  |  -  |
**413** | Request body exceeded the 8 KiB ceiling. Body is a &#x60;Problem&#x60; with &#x60;code: request_body_too_large&#x60;.  |  -  |
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

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

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

# The client must configure the authentication and authorization parameters
# in accordance with the API server security policy.
# Examples for each auth method are provided below, use the example that
# satisfies your auth use case.

# Configure Bearer authorization (JWT): operatorBearer
configuration = plexsphere.Configuration(
    access_token = os.environ["BEARER_TOKEN"]
)

# Configure API key authorization: sessionCookie
configuration.api_key['sessionCookie'] = os.environ["API_KEY"]

# Uncomment below to setup prefix (e.g. Bearer) for API key, if needed
# configuration.api_key_prefix['sessionCookie'] = 'Bearer'

# Enter a context with an instance of the API client
with plexsphere.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = plexsphere.CloudApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Cloud Credential identifier (UUIDv7). Bound on `/v1/cloud-credentials/{id}`, `/v1/cloud-credentials/{id}/revoke`, `/v1/cloud-credentials/{id}/clouds`, `/v1/cloud-credentials/{id}/credential-assignments`, and `/v1/cloud-credentials/{id}/clouds/{cloud_id}` for the operator-facing Cloud Credentials read, revoke, and usage-Cloud attach/detach surface. 
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
 **id** | **UUID**| Cloud Credential identifier (UUIDv7). Bound on &#x60;/v1/cloud-credentials/{id}&#x60;, &#x60;/v1/cloud-credentials/{id}/revoke&#x60;, &#x60;/v1/cloud-credentials/{id}/clouds&#x60;, &#x60;/v1/cloud-credentials/{id}/credential-assignments&#x60;, and &#x60;/v1/cloud-credentials/{id}/clouds/{cloud_id}&#x60; for the operator-facing Cloud Credentials read, revoke, and usage-Cloud attach/detach surface.  | 
 **cloud_credential_revoke_request** | [**CloudCredentialRevokeRequest**](CloudCredentialRevokeRequest.md)|  | 

### Return type

[**CloudCredentialResponse**](CloudCredentialResponse.md)

### Authorization

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

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
handler reads the row to resolve the owning Cloud Credential and
the consuming Project, runs the dual ReBAC gate described below,
then delegates to the Credential Assignment application service
which moves the assignment to the `revoked` state, tears down
the materialised binding, and appends a
`CredentialAssignmentRevoked` outbox event in a single
transaction. The `reason` from the body is recorded on the event
as an operator-supplied audit string.

The gate has two legs: `assign` on the owning Cloud Credential,
the object the assignment spends, and on its denial `deploy` on
the consuming Project. Either leg authorises the revocation; the
call is denied only when both deny. The second leg lets a
Project owner hand back a Credential they were granted without
holding the Credential-side `assign` relation.

Revocation is only legal from the `approved` state — any other
source state returns `409 illegal_transition`.


### Example

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

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

# The client must configure the authentication and authorization parameters
# in accordance with the API server security policy.
# Examples for each auth method are provided below, use the example that
# satisfies your auth use case.

# Configure Bearer authorization (JWT): operatorBearer
configuration = plexsphere.Configuration(
    access_token = os.environ["BEARER_TOKEN"]
)

# Configure API key authorization: sessionCookie
configuration.api_key['sessionCookie'] = os.environ["API_KEY"]

# Uncomment below to setup prefix (e.g. Bearer) for API key, if needed
# configuration.api_key_prefix['sessionCookie'] = 'Bearer'

# Enter a context with an instance of the API client
with plexsphere.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = plexsphere.CloudApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Credential Assignment identifier (UUIDv7). Bound on `/v1/credential-assignments/{id}/revoke` for the Credential Assignment revocation surface. 
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
 **id** | **UUID**| Credential Assignment identifier (UUIDv7). Bound on &#x60;/v1/credential-assignments/{id}/revoke&#x60; for the Credential Assignment revocation surface.  | 
 **credential_assignment_decision_request** | [**CredentialAssignmentDecisionRequest**](CredentialAssignmentDecisionRequest.md)|  | 

### Return type

[**CredentialAssignmentResponse**](CredentialAssignmentResponse.md)

### Authorization

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Credential Assignment revoked. Body is the metadata projection with &#x60;state: revoked&#x60; and &#x60;materialised: false&#x60;.  |  -  |
**400** | Malformed id or body. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;invalid_credential_assignment_id&#x60;, &#x60;invalid_body&#x60;, &#x60;invalid_decision_reason&#x60; }.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller holds neither &#x60;assign&#x60; on the owning Cloud Credential nor &#x60;deploy&#x60; on the consuming Project. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Credential Assignment not found. Body is a &#x60;Problem&#x60; with &#x60;code: credential_assignment_not_found&#x60;.  |  -  |
**409** | The assignment is not in a state from which revocation is legal. Body is a &#x60;Problem&#x60; with &#x60;code: illegal_transition&#x60;.  |  -  |
**413** | Request body exceeded the 8 KiB ceiling. Body is a &#x60;Problem&#x60; with &#x60;code: request_body_too_large&#x60;.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

