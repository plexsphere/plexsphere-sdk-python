# plexsphere.ArtifactsApi

All URIs are relative to *http://localhost*

Method | HTTP request | Description
------------- | ------------- | -------------
[**get_plexd_artifact**](ArtifactsApi.md#get_plexd_artifact) | **GET** /v1/artifacts/plexd/{version} | Fetch one indexed plexd release by version.
[**get_plexd_artifact_signature**](ArtifactsApi.md#get_plexd_artifact_signature) | **GET** /v1/artifacts/plexd/{version}/sigstore | Fetch the verbatim Sigstore bundle for a plexd release.
[**list_plexd_artifacts**](ArtifactsApi.md#list_plexd_artifacts) | **GET** /v1/artifacts/plexd | List indexed plexd releases.


# **get_plexd_artifact**
> PlexdArtifact get_plexd_artifact(version, authorization)

Fetch one indexed plexd release by version.

Returns the indexed plexd release line identified by `{version}`
— every per-architecture build with its recorded SHA-256
checksum, the lifecycle `support_status`, and the `verdict` the
supply-chain verification gate pronounced. The record is
read-only; indexing happens out of band against the upstream OCI
release stream. A version that has never been indexed surfaces as
`404 release_not_found`.

The verbatim Sigstore bundle attesting each build is NOT inlined
in this record; fetch it from the `{version}/sigstore` companion
endpoint so an operator can re-verify the attestation independently.


### Example


```python
import plexsphere
from plexsphere.models.plexd_artifact import PlexdArtifact
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
    api_instance = plexsphere.ArtifactsApi(api_client)
    version = 'version_example' # str | plexd release version the registry indexes — the upstream release tag, for example a semver tag such as `v1.4.2`. Bound on `/v1/artifacts/plexd/{version}` and its `.sigstore` companion for the read-only plexd release registry surface. 
    authorization = 'authorization_example' # str | `Bearer <access token>` — the operator bearer credential the request authenticates with. A missing, malformed, or rejected token surfaces as `401`. 

    try:
        # Fetch one indexed plexd release by version.
        api_response = api_instance.get_plexd_artifact(version, authorization)
        print("The response of ArtifactsApi->get_plexd_artifact:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling ArtifactsApi->get_plexd_artifact: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **version** | **str**| plexd release version the registry indexes — the upstream release tag, for example a semver tag such as &#x60;v1.4.2&#x60;. Bound on &#x60;/v1/artifacts/plexd/{version}&#x60; and its &#x60;.sigstore&#x60; companion for the read-only plexd release registry surface.  | 
 **authorization** | **str**| &#x60;Bearer &lt;access token&gt;&#x60; — the operator bearer credential the request authenticates with. A missing, malformed, or rejected token surfaces as &#x60;401&#x60;.  | 

### Return type

[**PlexdArtifact**](PlexdArtifact.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | plexd release found. |  -  |
**401** | Caller is not authenticated. |  -  |
**404** | plexd release not found. Body is a &#x60;Problem&#x60; with &#x60;code: release_not_found&#x60;.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **get_plexd_artifact_signature**
> bytes get_plexd_artifact_signature(version, authorization)

Fetch the verbatim Sigstore bundle for a plexd release.

Returns the raw, verbatim upstream Sigstore bundle the registry
cached for the plexd release identified by `{version}`, served
back byte-for-byte as `application/octet-stream`. The bundle is
the canonical keyless attestation an operator re-verifies
independently — pipe it into `cosign verify-blob` or a
`sigstore-go` verifier against the pinned release-signing
identity to confirm the build provenance without trusting this
server. A version that has never been indexed, or whose cached
bundle is absent, surfaces as `404 release_not_found`.


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
    api_instance = plexsphere.ArtifactsApi(api_client)
    version = 'version_example' # str | plexd release version the registry indexes — the upstream release tag, for example a semver tag such as `v1.4.2`. Bound on `/v1/artifacts/plexd/{version}` and its `.sigstore` companion for the read-only plexd release registry surface. 
    authorization = 'authorization_example' # str | `Bearer <access token>` — the operator bearer credential the request authenticates with. A missing, malformed, or rejected token surfaces as `401`. 

    try:
        # Fetch the verbatim Sigstore bundle for a plexd release.
        api_response = api_instance.get_plexd_artifact_signature(version, authorization)
        print("The response of ArtifactsApi->get_plexd_artifact_signature:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling ArtifactsApi->get_plexd_artifact_signature: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **version** | **str**| plexd release version the registry indexes — the upstream release tag, for example a semver tag such as &#x60;v1.4.2&#x60;. Bound on &#x60;/v1/artifacts/plexd/{version}&#x60; and its &#x60;.sigstore&#x60; companion for the read-only plexd release registry surface.  | 
 **authorization** | **str**| &#x60;Bearer &lt;access token&gt;&#x60; — the operator bearer credential the request authenticates with. A missing, malformed, or rejected token surfaces as &#x60;401&#x60;.  | 

### Return type

**bytes**

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/octet-stream, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | The verbatim Sigstore bundle bytes for the addressed release.  |  -  |
**401** | Caller is not authenticated. |  -  |
**404** | plexd release or its cached Sigstore bundle not found. Body is a &#x60;Problem&#x60; with &#x60;code: release_not_found&#x60;.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **list_plexd_artifacts**
> PlexdArtifactRefList list_plexd_artifacts(authorization, cursor=cursor, limit=limit)

List indexed plexd releases.

Returns a cursor-paginated page of indexed plexd release lines —
each with its lifecycle support status, the supply-chain
verification verdict, the per-architecture checksums, and the
index timestamps. The registry is platform-global; the handler
runs the platform read gate before the persistence read.

The pagination cursor is HMAC-signed and bound to the
per-(caller, pepper) pseudonym, so a cursor minted by one
principal cannot be replayed by another — the cross-caller
replay surfaces as `403 cursor_binding_mismatch`. A tampered
envelope or unknown version byte stays on `400 invalid_cursor`,
and `limit` is clamped to `[1, 200]` rather than rejected.


### Example


```python
import plexsphere
from plexsphere.models.plexd_artifact_ref_list import PlexdArtifactRefList
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
    api_instance = plexsphere.ArtifactsApi(api_client)
    authorization = 'authorization_example' # str | `Bearer <access token>` — the operator bearer credential the request authenticates with. A missing, malformed, or rejected token surfaces as `401`. 
    cursor = 'cursor_example' # str | Opaque continuation token returned by a previous call's `next_cursor`. The encoding is HMAC-signed by the server so a tampered cursor surfaces as `400`.  (optional)
    limit = 50 # int | Maximum number of items to return in a single page. The handler clamps the value to [1, 200] before forwarding it to the read service.  (optional) (default to 50)

    try:
        # List indexed plexd releases.
        api_response = api_instance.list_plexd_artifacts(authorization, cursor=cursor, limit=limit)
        print("The response of ArtifactsApi->list_plexd_artifacts:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling ArtifactsApi->list_plexd_artifacts: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **authorization** | **str**| &#x60;Bearer &lt;access token&gt;&#x60; — the operator bearer credential the request authenticates with. A missing, malformed, or rejected token surfaces as &#x60;401&#x60;.  | 
 **cursor** | **str**| Opaque continuation token returned by a previous call&#39;s &#x60;next_cursor&#x60;. The encoding is HMAC-signed by the server so a tampered cursor surfaces as &#x60;400&#x60;.  | [optional] 
 **limit** | **int**| Maximum number of items to return in a single page. The handler clamps the value to [1, 200] before forwarding it to the read service.  | [optional] [default to 50]

### Return type

[**PlexdArtifactRefList**](PlexdArtifactRefList.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Page of indexed plexd releases. |  -  |
**400** | Invalid query parameters — typically a tampered or malformed cursor or an out-of-range &#x60;limit&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to read the artifact registry (body is a &#x60;PermissionDenied&#x60; problem) OR the pagination cursor was minted by a different caller and the per-(caller, pepper) HMAC binding rejected the replay (body is a &#x60;Problem&#x60; with &#x60;code &#x3D; cursor_binding_mismatch&#x60;).  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

