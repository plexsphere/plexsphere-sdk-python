# plexsphere.BlueprintCatalogApi

All URIs are relative to *http://localhost*

Method | HTTP request | Description
------------- | ------------- | -------------
[**browse_catalog_source**](BlueprintCatalogApi.md#browse_catalog_source) | **GET** /v1/blueprint-catalogs/{id}/blueprints | Browse the Blueprints a catalog source offers.
[**deregister_catalog_source**](BlueprintCatalogApi.md#deregister_catalog_source) | **DELETE** /v1/blueprint-catalogs/{id} | Deregister a Blueprint catalog source.
[**get_catalog_source**](BlueprintCatalogApi.md#get_catalog_source) | **GET** /v1/blueprint-catalogs/{id} | Fetch a Blueprint catalog source by identifier.
[**import_from_catalog_source**](BlueprintCatalogApi.md#import_from_catalog_source) | **POST** /v1/blueprint-catalogs/{id}/import | Import Blueprints from a catalog source.
[**list_catalog_sources**](BlueprintCatalogApi.md#list_catalog_sources) | **GET** /v1/blueprint-catalogs | List the registered Blueprint catalog sources.
[**register_catalog_source**](BlueprintCatalogApi.md#register_catalog_source) | **POST** /v1/blueprint-catalogs | Register a Blueprint catalog source.


# **browse_catalog_source**
> CatalogBrowseResponse browse_catalog_source(id)

Browse the Blueprints a catalog source offers.

Resolves the registered catalog source's OCI bundle and returns
the Blueprints it offers, without importing anything. This is a
read-through: nothing is persisted, so the scope-parent `read`
check is the only gate. The handler resolves the source first to
learn its scope. A missing source surfaces as
`404 catalog_source_not_found`; a missing OCI artifact surfaces
as `404 catalog_artifact_not_found`; a transient or malformed
upstream registry surfaces as `502 catalog_upstream_error`.


### Example


```python
import plexsphere
from plexsphere.models.catalog_browse_response import CatalogBrowseResponse
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
    api_instance = plexsphere.BlueprintCatalogApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Catalog source identifier (UUIDv7). Bound on the `/v1/blueprint-catalogs/{id}` family — get, deregister, browse, and import. 

    try:
        # Browse the Blueprints a catalog source offers.
        api_response = api_instance.browse_catalog_source(id)
        print("The response of BlueprintCatalogApi->browse_catalog_source:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling BlueprintCatalogApi->browse_catalog_source: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Catalog source identifier (UUIDv7). Bound on the &#x60;/v1/blueprint-catalogs/{id}&#x60; family — get, deregister, browse, and import.  | 

### Return type

[**CatalogBrowseResponse**](CatalogBrowseResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Blueprints the catalog source offers. |  -  |
**400** | Malformed &#x60;{id}&#x60;. Body is a &#x60;Problem&#x60; with &#x60;code: invalid_catalog_source_id&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to browse the addressed catalog source. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | The catalog source or its upstream OCI artifact was not found. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;catalog_source_not_found&#x60;, &#x60;catalog_artifact_not_found&#x60; }.  |  -  |
**502** | The upstream registry was unreachable or returned a malformed bundle. Body is a &#x60;Problem&#x60; with &#x60;code: catalog_upstream_error&#x60;.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **deregister_catalog_source**
> deregister_catalog_source(id)

Deregister a Blueprint catalog source.

Deregisters the catalog source identified by `{id}`.
Deregistration orphans, rather than deletes, any Blueprints
already imported from the source: their provenance catalog id is
cleared while the imported versions stay in place. The handler
resolves the source first to learn its scope, then authorises the
caller for `manage` on the scope parent (`platform#manage` or
`domain#manage`). A missing source surfaces as
`404 catalog_source_not_found`.


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
    api_instance = plexsphere.BlueprintCatalogApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Catalog source identifier (UUIDv7). Bound on the `/v1/blueprint-catalogs/{id}` family — get, deregister, browse, and import. 

    try:
        # Deregister a Blueprint catalog source.
        api_instance.deregister_catalog_source(id)
    except Exception as e:
        print("Exception when calling BlueprintCatalogApi->deregister_catalog_source: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Catalog source identifier (UUIDv7). Bound on the &#x60;/v1/blueprint-catalogs/{id}&#x60; family — get, deregister, browse, and import.  | 

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
**204** | Catalog source deregistered. |  -  |
**400** | Malformed &#x60;{id}&#x60;. Body is a &#x60;Problem&#x60; with &#x60;code: invalid_catalog_source_id&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to deregister the addressed catalog source. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Catalog source not found. Body is a &#x60;Problem&#x60; with &#x60;code: catalog_source_not_found&#x60;.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **get_catalog_source**
> CatalogSourceResponse get_catalog_source(id)

Fetch a Blueprint catalog source by identifier.

Returns the catalog source identified by `{id}`. The handler
resolves the source first to learn its scope, then authorises the
caller for `read` on the scope parent — the platform singleton
(`platform#read`) for a catalog-global source, the owning Domain
(`domain#read`) for a Domain-scoped source. A missing source
surfaces as `404 catalog_source_not_found`; an unauthorised
caller therefore receives `403` only after the source was found.


### Example


```python
import plexsphere
from plexsphere.models.catalog_source_response import CatalogSourceResponse
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
    api_instance = plexsphere.BlueprintCatalogApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Catalog source identifier (UUIDv7). Bound on the `/v1/blueprint-catalogs/{id}` family — get, deregister, browse, and import. 

    try:
        # Fetch a Blueprint catalog source by identifier.
        api_response = api_instance.get_catalog_source(id)
        print("The response of BlueprintCatalogApi->get_catalog_source:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling BlueprintCatalogApi->get_catalog_source: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Catalog source identifier (UUIDv7). Bound on the &#x60;/v1/blueprint-catalogs/{id}&#x60; family — get, deregister, browse, and import.  | 

### Return type

[**CatalogSourceResponse**](CatalogSourceResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Catalog source found. |  -  |
**400** | Malformed &#x60;{id}&#x60;. Body is a &#x60;Problem&#x60; with &#x60;code: invalid_catalog_source_id&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to read the addressed catalog source. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Catalog source not found. Body is a &#x60;Problem&#x60; with &#x60;code: catalog_source_not_found&#x60;.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **import_from_catalog_source**
> ImportResult import_from_catalog_source(id, import_request)

Import Blueprints from a catalog source.

Imports a selected subset of, or all of, the Blueprints a
registered catalog source offers into the Blueprint Catalog. The
request supplies exactly one of `slugs` (a non-empty subset) or
`all: true`; supplying neither, both, or an empty `slugs` array
surfaces as `400 invalid_import_request`.

The handler resolves the source first to learn its scope, then
authorises the caller for `manage` on the scope parent
(`platform#manage` or `domain#manage`). Import is per-Blueprint:
the response `outcomes` array reports each requested slug's
status — `imported`, `unchanged`, `drift`, `conflict`, or
`error`. A per-Blueprint conflict or fetch failure does not abort
the batch; only a whole-batch precondition failure (the source
not existing, or the registry listing failing on an `all` import)
returns an error response.


### Example


```python
import plexsphere
from plexsphere.models.import_request import ImportRequest
from plexsphere.models.import_result import ImportResult
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
    api_instance = plexsphere.BlueprintCatalogApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Catalog source identifier (UUIDv7). Bound on the `/v1/blueprint-catalogs/{id}` family — get, deregister, browse, and import. 
    import_request = {"slugs":["hetzner-server"]} # ImportRequest | 

    try:
        # Import Blueprints from a catalog source.
        api_response = api_instance.import_from_catalog_source(id, import_request)
        print("The response of BlueprintCatalogApi->import_from_catalog_source:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling BlueprintCatalogApi->import_from_catalog_source: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Catalog source identifier (UUIDv7). Bound on the &#x60;/v1/blueprint-catalogs/{id}&#x60; family — get, deregister, browse, and import.  | 
 **import_request** | [**ImportRequest**](ImportRequest.md)|  | 

### Return type

[**ImportResult**](ImportResult.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Per-Blueprint import outcomes. |  -  |
**400** | The import selection was invalid (neither, both, or an empty &#x60;slugs&#x60;/&#x60;all&#x60;) or the body was undecodable. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;invalid_import_request&#x60;, &#x60;invalid_body&#x60; }.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to import from the addressed catalog source. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Catalog source not found. Body is a &#x60;Problem&#x60; with &#x60;code: catalog_source_not_found&#x60;.  |  -  |
**413** | Request body exceeded the import-request ceiling enforced by the handler.  |  -  |
**502** | An &#x60;all&#x60; import could not enumerate the upstream bundle. Body is a &#x60;Problem&#x60; with &#x60;code: catalog_upstream_error&#x60;.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **list_catalog_sources**
> CatalogSourceList list_catalog_sources()

List the registered Blueprint catalog sources.

Returns every registered catalog source the caller is
authorised to see. The set is small, operator-curated
configuration returned whole, with no pagination. Per-row
visibility is layered on top: a source whose scope-parent `read`
relation the caller does not hold is filtered out, so the
response items are a subset of the registered set.


### Example


```python
import plexsphere
from plexsphere.models.catalog_source_list import CatalogSourceList
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
    api_instance = plexsphere.BlueprintCatalogApi(api_client)

    try:
        # List the registered Blueprint catalog sources.
        api_response = api_instance.list_catalog_sources()
        print("The response of BlueprintCatalogApi->list_catalog_sources:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling BlueprintCatalogApi->list_catalog_sources: %s\n" % e)
```



### Parameters

This endpoint does not need any parameter.

### Return type

[**CatalogSourceList**](CatalogSourceList.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Registered catalog sources. |  -  |
**401** | Caller is not authenticated. |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **register_catalog_source**
> CatalogSourceResponse register_catalog_source(catalog_source_create_request)

Register a Blueprint catalog source.

Registers an external OCI catalog the importer can later browse
and import Blueprints from. The aggregate enforces every
invariant: a non-empty `name`, an `oci_reference` pinned to
exactly one of a mutable `tag` or an immutable `digest`, a
well-formed `verification` policy, and a `tracking` policy whose
`track-tag` mode requires a tag-pinned reference.

The handler authorises the caller against the source's scope
parent BEFORE invoking the service: a catalog-global source (no
`domain_id`) requires the platform-level `manage` relation
(`platform#manage`); a Domain-scoped source requires `manage` on
the owning Domain (`domain#manage`). The body is decoded before
the authz check because the scope to gate on is carried in the
body, but the check runs before the service persists anything.


### Example


```python
import plexsphere
from plexsphere.models.catalog_source_create_request import CatalogSourceCreateRequest
from plexsphere.models.catalog_source_response import CatalogSourceResponse
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
    api_instance = plexsphere.BlueprintCatalogApi(api_client)
    catalog_source_create_request = {"name":"Platform catalog","oci_reference":{"registry":"ghcr.io","repository":"plexsphere/catalog","tag":"v1.2.3"},"verification":{"mode":"pinned","identity_san":"^https://github\\.com/plexsphere/.*$","issuer":"https://token.actions.githubusercontent.com"},"tracking":{"mode":"pinned"},"status":"active"} # CatalogSourceCreateRequest | 

    try:
        # Register a Blueprint catalog source.
        api_response = api_instance.register_catalog_source(catalog_source_create_request)
        print("The response of BlueprintCatalogApi->register_catalog_source:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling BlueprintCatalogApi->register_catalog_source: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **catalog_source_create_request** | [**CatalogSourceCreateRequest**](CatalogSourceCreateRequest.md)|  | 

### Return type

[**CatalogSourceResponse**](CatalogSourceResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**201** | Catalog source registered. |  * Location - Absolute path of the issued Session for the single-Session read.  <br>  |
**400** | Aggregate rejected the body — a malformed &#x60;name&#x60;, an invalid &#x60;oci_reference&#x60;, &#x60;verification&#x60;, &#x60;tracking&#x60;, or &#x60;credential_ref&#x60;, or an undecodable request body. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;invalid_catalog_source&#x60;, &#x60;invalid_oci_reference&#x60;, &#x60;invalid_verification_policy&#x60;, &#x60;invalid_tracking_policy&#x60;, &#x60;invalid_credential_ref&#x60;, &#x60;invalid_body&#x60; }.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to register a catalog source in the requested scope. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**413** | Request body exceeded the catalog-source authorship ceiling enforced by the handler.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

