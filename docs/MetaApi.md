# plexsphere.MetaApi

All URIs are relative to *http://localhost*

Method | HTTP request | Description
------------- | ------------- | -------------
[**get_docs**](MetaApi.md#get_docs) | **GET** /v1/docs | Serve the embedded ReDoc UI for the v1 OpenAPI document.
[**get_docs_asset**](MetaApi.md#get_docs_asset) | **GET** /v1/docs/assets/{asset} | Serve a vendored ReDoc bundle asset.
[**get_health**](MetaApi.md#get_health) | **GET** /v1/health | Report the process health and sub-component checks.
[**get_open_api**](MetaApi.md#get_open_api) | **GET** /v1/openapi.json | Serve the live OpenAPI document describing this API.
[**get_version**](MetaApi.md#get_version) | **GET** /v1/version | Return build metadata for the running binary.


# **get_docs**
> str get_docs()

Serve the embedded ReDoc UI for the v1 OpenAPI document.

Returns the self-contained HTML page that bootstraps the ReDoc
renderer against `/v1/openapi.json`.
The bundle is vendored into the binary so the documentation UI
works in air-gapped deployments without reaching out to a CDN.


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
    api_instance = plexsphere.MetaApi(api_client)

    try:
        # Serve the embedded ReDoc UI for the v1 OpenAPI document.
        api_response = api_instance.get_docs()
        print("The response of MetaApi->get_docs:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling MetaApi->get_docs: %s\n" % e)
```



### Parameters

This endpoint does not need any parameter.

### Return type

**str**

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: text/html, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | ReDoc HTML shell. |  -  |
**404** | Endpoint not found. |  -  |
**500** | Internal server error rendering the docs shell. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **get_docs_asset**
> str get_docs_asset(asset)

Serve a vendored ReDoc bundle asset.

Serves the requested asset from the vendored ReDoc bundle that
backs the embedded UI at `/v1/docs`.
The handler restricts the `asset` parameter to the bundle's
allow-list so the endpoint cannot be used as a generic file
server.


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
    api_instance = plexsphere.MetaApi(api_client)
    asset = 'asset_example' # str | Bundle asset filename (e.g. the ReDoc standalone JavaScript bundle). The handler matches the value against the vendored allow-list and returns 404 for anything outside it. 

    try:
        # Serve a vendored ReDoc bundle asset.
        api_response = api_instance.get_docs_asset(asset)
        print("The response of MetaApi->get_docs_asset:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling MetaApi->get_docs_asset: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **asset** | **str**| Bundle asset filename (e.g. the ReDoc standalone JavaScript bundle). The handler matches the value against the vendored allow-list and returns 404 for anything outside it.  | 

### Return type

**str**

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/javascript, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | ReDoc bundle asset. |  -  |
**404** | Asset not found in the vendored bundle. |  -  |
**500** | Internal server error reading the bundle asset. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **get_health**
> HealthStatus get_health()

Report the process health and sub-component checks.

### Example


```python
import plexsphere
from plexsphere.models.health_status import HealthStatus
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
    api_instance = plexsphere.MetaApi(api_client)

    try:
        # Report the process health and sub-component checks.
        api_response = api_instance.get_health()
        print("The response of MetaApi->get_health:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling MetaApi->get_health: %s\n" % e)
```



### Parameters

This endpoint does not need any parameter.

### Return type

[**HealthStatus**](HealthStatus.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Current health snapshot. |  -  |
**404** | Endpoint not found. |  -  |
**500** | Internal server error while computing health. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **get_open_api**
> Dict[str, object] get_open_api()

Serve the live OpenAPI document describing this API.

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
    api_instance = plexsphere.MetaApi(api_client)

    try:
        # Serve the live OpenAPI document describing this API.
        api_response = api_instance.get_open_api()
        print("The response of MetaApi->get_open_api:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling MetaApi->get_open_api: %s\n" % e)
```



### Parameters

This endpoint does not need any parameter.

### Return type

**Dict[str, object]**

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | The OpenAPI document as JSON. |  -  |
**404** | Endpoint not found. |  -  |
**500** | Internal server error rendering the spec. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **get_version**
> VersionInfo get_version()

Return build metadata for the running binary.

### Example


```python
import plexsphere
from plexsphere.models.version_info import VersionInfo
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
    api_instance = plexsphere.MetaApi(api_client)

    try:
        # Return build metadata for the running binary.
        api_response = api_instance.get_version()
        print("The response of MetaApi->get_version:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling MetaApi->get_version: %s\n" % e)
```



### Parameters

This endpoint does not need any parameter.

### Return type

[**VersionInfo**](VersionInfo.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Build metadata. |  -  |
**404** | Endpoint not found. |  -  |
**500** | Internal server error reading build metadata. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

