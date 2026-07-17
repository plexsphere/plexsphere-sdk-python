# plexsphere.LabelsApi

All URIs are relative to *http://localhost*

Method | HTTP request | Description
------------- | ------------- | -------------
[**create_label_definition**](LabelsApi.md#create_label_definition) | **POST** /v1/label-definitions | Create a Label Definition at a specific scope.
[**delete_label_definition**](LabelsApi.md#delete_label_definition) | **DELETE** /v1/label-definitions/{id} | Delete a Label Definition.
[**delete_object_label**](LabelsApi.md#delete_object_label) | **DELETE** /v1/objects/{kind}/{id}/labels | Remove a Label Assignment from an object.
[**get_label_definition**](LabelsApi.md#get_label_definition) | **GET** /v1/label-definitions/{id} | Fetch a Label Definition by identifier.
[**list_label_definitions**](LabelsApi.md#list_label_definitions) | **GET** /v1/label-definitions | List Label Definitions in a scope.
[**list_object_labels**](LabelsApi.md#list_object_labels) | **GET** /v1/objects/{kind}/{id}/labels | List Label Assignments attached to an object.
[**preview_label_selector**](LabelsApi.md#preview_label_selector) | **POST** /v1/labels/selectors/preview | Parse a label selector and return its AST or errors inline.
[**put_object_label**](LabelsApi.md#put_object_label) | **PUT** /v1/objects/{kind}/{id}/labels | Upsert a Label Assignment on an object.
[**search_objects_by_label**](LabelsApi.md#search_objects_by_label) | **POST** /v1/objects/search | Search objects by Label selector, filtered to the caller&#39;s access.
[**update_label_definition**](LabelsApi.md#update_label_definition) | **PATCH** /v1/label-definitions/{id} | Update mutable fields on a Label Definition.


# **create_label_definition**
> LabelDefinition create_label_definition(label_definition_request)

Create a Label Definition at a specific scope.

Creates a new Label Definition at Platform, Domain, or Project
scope. The services layer performs the `manage` ReBAC check on
the scope-object and emits exactly one audit entry on the
outcome.


### Example

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

```python
import plexsphere
from plexsphere.models.label_definition import LabelDefinition
from plexsphere.models.label_definition_request import LabelDefinitionRequest
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
    api_instance = plexsphere.LabelsApi(api_client)
    label_definition_request = plexsphere.LabelDefinitionRequest() # LabelDefinitionRequest | 

    try:
        # Create a Label Definition at a specific scope.
        api_response = api_instance.create_label_definition(label_definition_request)
        print("The response of LabelsApi->create_label_definition:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling LabelsApi->create_label_definition: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **label_definition_request** | [**LabelDefinitionRequest**](LabelDefinitionRequest.md)|  | 

### Return type

[**LabelDefinition**](LabelDefinition.md)

### Authorization

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**201** | Label Definition created. |  * Location - Canonical read URL of the created resource — &#x60;/v1/domains/{domain_id}/incidents/{incident_id}&#x60;.  <br>  |
**400** | Invalid body or scope mismatch. |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to create Label Definitions in the requested scope. Body is a &#x60;PermissionDenied&#x60; problem carrying the ReBAC denial &#x60;reason&#x60;, &#x60;relation_path&#x60;, and &#x60;correlation_id&#x60;.  |  -  |
**409** | Conflict — a Label Definition with the same &#x60;(scope_id, local_key)&#x60; already exists.  |  -  |
**422** | Aggregate invariant violated (reserved key, invalid value schema, …).  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **delete_label_definition**
> delete_label_definition(id)

Delete a Label Definition.

Deletes the Label Definition identified by `{id}` subject to its
`on_delete` policy (block / cascade / orphan). A `block` policy
with outstanding Assignments returns 409 `assignments_exist`. A
`cascade` policy deletes every referencing Assignment in the same
transaction. An `orphan` policy detaches the Assignments — they
stay readable per object via
`GET /v1/objects/{kind}/{id}/labels` without a `definition_id`,
but drop out of every Definition-scoped read — then deletes the
Definition. All three policies return 204 on success.


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
    api_instance = plexsphere.LabelsApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Label Definition identifier (UUIDv7).

    try:
        # Delete a Label Definition.
        api_instance.delete_label_definition(id)
    except Exception as e:
        print("Exception when calling LabelsApi->delete_label_definition: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Label Definition identifier (UUIDv7). | 

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
**204** | Label Definition deleted. |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to delete the Definition. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Label Definition not found. |  -  |
**409** | Block policy refused the delete because Assignments still reference the Definition.  |  -  |
**422** | Aggregate invariant refused the delete (e.g. system-seed definition).  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **delete_object_label**
> delete_object_label(kind, id, qualified_key)

Remove a Label Assignment from an object.

Deletes the Label Assignment linking the object to the
qualified_key. Idempotent — returns 204 whether or not an
Assignment was physically removed.


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
    api_instance = plexsphere.LabelsApi(api_client)
    kind = 'kind_example' # str | Lowercase object-kind discriminator.
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Object identifier (UUIDv7).
    qualified_key = 'qualified_key_example' # str | Fully-qualified Label key (e.g. `platform/env` or `acme:checkout/owner`). DECISION: addressed by qualified_key rather than definition_id so the client does not need a Definition lookup to issue the delete. 

    try:
        # Remove a Label Assignment from an object.
        api_instance.delete_object_label(kind, id, qualified_key)
    except Exception as e:
        print("Exception when calling LabelsApi->delete_object_label: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **kind** | **str**| Lowercase object-kind discriminator. | 
 **id** | **UUID**| Object identifier (UUIDv7). | 
 **qualified_key** | **str**| Fully-qualified Label key (e.g. &#x60;platform/env&#x60; or &#x60;acme:checkout/owner&#x60;). DECISION: addressed by qualified_key rather than definition_id so the client does not need a Definition lookup to issue the delete.  | 

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
**204** | Assignment removed (or already absent). |  -  |
**400** | Invalid path parameters or qualified_key. |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Referenced Label Definition does not exist. |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **get_label_definition**
> LabelDefinition get_label_definition(id)

Fetch a Label Definition by identifier.

Returns the Label Definition identified by `{id}`.

The handler deliberately omits a ReBAC `Check` call. Visibility
is enforced at the repo layer: Definitions the caller cannot
see return `ErrDefinitionNotVisible` which surfaces as 404, so
this endpoint never emits 403.


### Example

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

```python
import plexsphere
from plexsphere.models.label_definition import LabelDefinition
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
    api_instance = plexsphere.LabelsApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Label Definition identifier (UUIDv7).

    try:
        # Fetch a Label Definition by identifier.
        api_response = api_instance.get_label_definition(id)
        print("The response of LabelsApi->get_label_definition:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling LabelsApi->get_label_definition: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Label Definition identifier (UUIDv7). | 

### Return type

[**LabelDefinition**](LabelDefinition.md)

### Authorization

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Label Definition found. |  -  |
**401** | Caller is not authenticated. |  -  |
**404** | Label Definition not found or not visible to the caller. |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **list_label_definitions**
> LabelDefinitionList list_label_definitions(scope, cursor=cursor, limit=limit)

List Label Definitions in a scope.

Returns Label Definitions in deterministic order with cursor
pagination. The scope discriminator names the scope the listing
targets — platform, a specific Domain, or a specific Project.


### Example

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

```python
import plexsphere
from plexsphere.models.label_definition_list import LabelDefinitionList
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
    api_instance = plexsphere.LabelsApi(api_client)
    scope = 'scope_example' # str | Scope selector. Accepts the literal `platform`, or `domain:<uuid>`, or `project:<uuid>`. DECISION: scope is a single string rather than a pair of query params because the three forms are mutually exclusive and the SpiceDB scope-object derivation treats them as a single coordinate. 
    cursor = 'cursor_example' # str | Opaque continuation token returned by a previous call's `next_cursor`. The encoding is HMAC-signed by the server so a tampered cursor surfaces as `400`.  (optional)
    limit = 50 # int | Maximum number of items to return in a single page. A value outside [1, 200] is rejected with a `400` Problem rather than silently clamped.  (optional) (default to 50)

    try:
        # List Label Definitions in a scope.
        api_response = api_instance.list_label_definitions(scope, cursor=cursor, limit=limit)
        print("The response of LabelsApi->list_label_definitions:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling LabelsApi->list_label_definitions: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **scope** | **str**| Scope selector. Accepts the literal &#x60;platform&#x60;, or &#x60;domain:&lt;uuid&gt;&#x60;, or &#x60;project:&lt;uuid&gt;&#x60;. DECISION: scope is a single string rather than a pair of query params because the three forms are mutually exclusive and the SpiceDB scope-object derivation treats them as a single coordinate.  | 
 **cursor** | **str**| Opaque continuation token returned by a previous call&#39;s &#x60;next_cursor&#x60;. The encoding is HMAC-signed by the server so a tampered cursor surfaces as &#x60;400&#x60;.  | [optional] 
 **limit** | **int**| Maximum number of items to return in a single page. A value outside [1, 200] is rejected with a &#x60;400&#x60; Problem rather than silently clamped.  | [optional] [default to 50]

### Return type

[**LabelDefinitionList**](LabelDefinitionList.md)

### Authorization

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Page of Label Definitions. |  -  |
**400** | Invalid query parameters. |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to read Definitions in this scope. Body is a &#x60;PermissionDenied&#x60; problem carrying the ReBAC denial &#x60;reason&#x60; (e.g. &#x60;insufficient_relation&#x60;), the traversed &#x60;relation_path&#x60;, and the &#x60;correlation_id&#x60;.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **list_object_labels**
> LabelAssignmentList list_object_labels(kind, id)

List Label Assignments attached to an object.

Returns every Label Assignment attached to the object
identified by `(kind, id)`. Orphaned Assignments (parent
Definition deleted with `on_delete=orphan`) are INCLUDED, each
without a `definition_id`; they are excluded only from the
selector and effective-set reads.


### Example

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

```python
import plexsphere
from plexsphere.models.label_assignment_list import LabelAssignmentList
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
    api_instance = plexsphere.LabelsApi(api_client)
    kind = 'kind_example' # str | Lowercase object-kind discriminator (resource, node, project, domain, workload, network, …). 
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Object identifier (UUIDv7).

    try:
        # List Label Assignments attached to an object.
        api_response = api_instance.list_object_labels(kind, id)
        print("The response of LabelsApi->list_object_labels:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling LabelsApi->list_object_labels: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **kind** | **str**| Lowercase object-kind discriminator (resource, node, project, domain, workload, network, …).  | 
 **id** | **UUID**| Object identifier (UUIDv7). | 

### Return type

[**LabelAssignmentList**](LabelAssignmentList.md)

### Authorization

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Assignments attached to the object. |  -  |
**400** | Invalid path parameters. |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized (&#x60;maintainer&#x60; on the object). Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **preview_label_selector**
> LabelSelectorPreviewResponse preview_label_selector(label_selector_preview_request)

Parse a label selector and return its AST or errors inline.

Parses the supplied selector expression and either returns the
parsed AST or a list of parse errors with byte offsets. Preview
never raises on syntax failure — the 200 body carries an
`errors` array so clients can render inline squiggles without
distinguishing HTTP status codes.


### Example

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

```python
import plexsphere
from plexsphere.models.label_selector_preview_request import LabelSelectorPreviewRequest
from plexsphere.models.label_selector_preview_response import LabelSelectorPreviewResponse
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
    api_instance = plexsphere.LabelsApi(api_client)
    label_selector_preview_request = plexsphere.LabelSelectorPreviewRequest() # LabelSelectorPreviewRequest | 

    try:
        # Parse a label selector and return its AST or errors inline.
        api_response = api_instance.preview_label_selector(label_selector_preview_request)
        print("The response of LabelsApi->preview_label_selector:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling LabelsApi->preview_label_selector: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **label_selector_preview_request** | [**LabelSelectorPreviewRequest**](LabelSelectorPreviewRequest.md)|  | 

### Return type

[**LabelSelectorPreviewResponse**](LabelSelectorPreviewResponse.md)

### Authorization

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Parsed AST or inline error report. |  -  |
**400** | Invalid body (not a LabelSelectorPreviewRequest). |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to preview selectors. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **put_object_label**
> LabelAssignment put_object_label(kind, id, label_assignment_request)

Upsert a Label Assignment on an object.

Upserts the Label Assignment linking the object to the supplied
Definition with the supplied value. The services layer performs
a dual ReBAC check — `assign` on the Definition AND `maintainer`
on the target object — and emits a single audit entry.


### Example

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

```python
import plexsphere
from plexsphere.models.label_assignment import LabelAssignment
from plexsphere.models.label_assignment_request import LabelAssignmentRequest
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
    api_instance = plexsphere.LabelsApi(api_client)
    kind = 'kind_example' # str | Lowercase object-kind discriminator.
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Object identifier (UUIDv7).
    label_assignment_request = plexsphere.LabelAssignmentRequest() # LabelAssignmentRequest | 

    try:
        # Upsert a Label Assignment on an object.
        api_response = api_instance.put_object_label(kind, id, label_assignment_request)
        print("The response of LabelsApi->put_object_label:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling LabelsApi->put_object_label: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **kind** | **str**| Lowercase object-kind discriminator. | 
 **id** | **UUID**| Object identifier (UUIDv7). | 
 **label_assignment_request** | [**LabelAssignmentRequest**](LabelAssignmentRequest.md)|  | 

### Return type

[**LabelAssignment**](LabelAssignment.md)

### Authorization

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Assignment upserted. |  -  |
**400** | Invalid path parameters or body. |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized — &#x60;assign&#x60; on the Definition or &#x60;maintainer&#x60; on the target object was denied. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Referenced Label Definition does not exist. |  -  |
**409** | Immutable-Definition replacement OR per-object cardinality ceiling (64) exceeded.  |  -  |
**422** | Aggregate invariant violated (value schema, scope mismatch, reserved key).  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **search_objects_by_label**
> ObjectSearchResponse search_objects_by_label(object_search_request)

Search objects by Label selector, filtered to the caller's access.

Resolves a Label `selector` to the objects that carry the
matching Labels within `scope`, then filters each match by a
ReBAC `relation` check for the authenticated principal — the
direct answer to "which objects carry Label X=Y that I can
access?". It is the browser/API counterpart of `plexctl label
object search` and combines the Label axis (the selector) with
the access axis (the relation) that previously required a manual
`lookup-resources | jq` intersection.

Scope is mandatory: `scope` names the tenancy universe
(`platform`, `domain`, or `project`) and `scope_id` carries the
Domain/Project UUID (omitted for `platform`). The result is
mixed-kind unless narrowed by the optional `kind` filter.

DECISION: pagination is filter-after-page. `limit` bounds the
PRE-filter page read from the selector index; the ReBAC filter
then drops matches the caller cannot access, so a returned page
may hold FEWER than `limit` items even when `next_cursor` is
present. Callers paginate until `next_cursor` is absent rather
than until a short page. Over-fetch-to-fill is intentionally not
implemented.

DECISION: per-object denials are silent — a match the caller
cannot reach via `relation` is omitted from `items`, never
surfaced as a 403. This mirrors `/v1/authz/lookup-resources`:
the enumeration is data, and an empty `items` is a normal
answer. There is no endpoint-level relation gate; the per-match
check IS the gate, so the search cannot enumerate objects the
caller has no access to.


### Example

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

```python
import plexsphere
from plexsphere.models.object_search_request import ObjectSearchRequest
from plexsphere.models.object_search_response import ObjectSearchResponse
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
    api_instance = plexsphere.LabelsApi(api_client)
    object_search_request = {"selector":"env=production","relation":"read","scope":"domain","scope_id":"0190a8b8-a0c0-7a0a-8a0a-a0a0a0a0a0a1","kind":"project"} # ObjectSearchRequest | 

    try:
        # Search objects by Label selector, filtered to the caller's access.
        api_response = api_instance.search_objects_by_label(object_search_request)
        print("The response of LabelsApi->search_objects_by_label:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling LabelsApi->search_objects_by_label: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **object_search_request** | [**ObjectSearchRequest**](ObjectSearchRequest.md)|  | 

### Return type

[**ObjectSearchResponse**](ObjectSearchResponse.md)

### Authorization

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Materialised page of object references the selector matched AND the caller can reach via &#x60;relation&#x60;. An empty &#x60;items&#x60; array is a normal answer.  |  -  |
**400** | Body could not be decoded, the selector failed to parse, or the (scope, scope_id) pair is invalid. Body is a &#x60;Problem&#x60; with &#x60;code&#x60; ∈ { &#x60;invalid_body&#x60;, &#x60;selector_syntax&#x60;, &#x60;scope_mismatch&#x60; }.  |  -  |
**401** | Caller is not authenticated. |  -  |
**413** | Request body exceeded the 64 KiB ceiling enforced by the handler.  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **update_label_definition**
> LabelDefinition update_label_definition(id, label_definition_update_request)

Update mutable fields on a Label Definition.

Updates the Label Definition identified by `{id}`. Immutable
Definitions reject value-schema changes with 409
`immutable_violation`.


### Example

* Bearer (JWT) Authentication (operatorBearer):
* Api Key Authentication (sessionCookie):

```python
import plexsphere
from plexsphere.models.label_definition import LabelDefinition
from plexsphere.models.label_definition_update_request import LabelDefinitionUpdateRequest
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
    api_instance = plexsphere.LabelsApi(api_client)
    id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Label Definition identifier (UUIDv7).
    label_definition_update_request = plexsphere.LabelDefinitionUpdateRequest() # LabelDefinitionUpdateRequest | 

    try:
        # Update mutable fields on a Label Definition.
        api_response = api_instance.update_label_definition(id, label_definition_update_request)
        print("The response of LabelsApi->update_label_definition:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling LabelsApi->update_label_definition: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **id** | **UUID**| Label Definition identifier (UUIDv7). | 
 **label_definition_update_request** | [**LabelDefinitionUpdateRequest**](LabelDefinitionUpdateRequest.md)|  | 

### Return type

[**LabelDefinition**](LabelDefinition.md)

### Authorization

[operatorBearer](../README.md#operatorBearer), [sessionCookie](../README.md#sessionCookie)

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Label Definition updated. |  -  |
**400** | Invalid body. |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to manage the Definition&#39;s scope. Body is a &#x60;PermissionDenied&#x60; problem.  |  -  |
**404** | Label Definition not found. |  -  |
**409** | Immutable Definition rejected a value-schema change.  |  -  |
**422** | Aggregate invariant violated. |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

