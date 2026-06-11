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
[**update_label_definition**](LabelsApi.md#update_label_definition) | **PATCH** /v1/label-definitions/{id} | Update mutable fields on a Label Definition.


# **create_label_definition**
> LabelDefinition create_label_definition(label_definition_request)

Create a Label Definition at a specific scope.

Creates a new Label Definition at Platform, Domain, or Project
scope. The services layer performs the `manage` ReBAC check on
the scope-object and emits exactly one audit entry on the
outcome.


### Example


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

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**201** | Label Definition created. |  -  |
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
with outstanding Assignments returns 409 `assignments-exist`
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

No authorization required

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

No authorization required

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

Returns the Label Definition identified by `{id}`
.

The handler deliberately omits a ReBAC `Check` call. Visibility
is enforced at the repo layer: Definitions the caller cannot
see return `ErrDefinitionNotVisible` which surfaces as 404, so
this endpoint never emits 403.


### Example


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

No authorization required

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
> LabelDefinitionListResponse list_label_definitions(scope, cursor=cursor, limit=limit)

List Label Definitions in a scope.

Returns Label Definitions in deterministic order with cursor
pagination. The scope discriminator names the scope the listing
targets — platform, a specific Domain, or a specific Project
.


### Example


```python
import plexsphere
from plexsphere.models.label_definition_list_response import LabelDefinitionListResponse
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
    api_instance = plexsphere.LabelsApi(api_client)
    scope = 'scope_example' # str | Scope selector. Accepts the literal `platform`, or `domain:<uuid>`, or `project:<uuid>`. DECISION: scope is a single string rather than a pair of query params because the three forms are mutually exclusive and the SpiceDB scope-object derivation treats them as a single coordinate . 
    cursor = 'cursor_example' # str | Opaque continuation token returned by a previous call's `next_cursor`.  (optional)
    limit = 50 # int | Maximum number of items to return in a single page .  (optional) (default to 50)

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
 **scope** | **str**| Scope selector. Accepts the literal &#x60;platform&#x60;, or &#x60;domain:&lt;uuid&gt;&#x60;, or &#x60;project:&lt;uuid&gt;&#x60;. DECISION: scope is a single string rather than a pair of query params because the three forms are mutually exclusive and the SpiceDB scope-object derivation treats them as a single coordinate .  | 
 **cursor** | **str**| Opaque continuation token returned by a previous call&#39;s &#x60;next_cursor&#x60;.  | [optional] 
 **limit** | **int**| Maximum number of items to return in a single page .  | [optional] [default to 50]

### Return type

[**LabelDefinitionListResponse**](LabelDefinitionListResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Page of Label Definitions. |  -  |
**400** | Invalid query parameters. |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller is not authorized to read Definitions in this scope. Body is a &#x60;PermissionDenied&#x60; problem carrying the ReBAC denial &#x60;reason&#x60; (e.g. &#x60;insufficient_relation&#x60;), the traversed &#x60;relation_path&#x60;, and the &#x60;correlation_id&#x60; .  |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **list_object_labels**
> LabelAssignmentList list_object_labels(kind, id)

List Label Assignments attached to an object.

Returns every Label Assignment attached to the object
identified by `(kind, id)`. Orphaned Assignments (parent
Definition deleted with `on_delete=orphan`) are excluded
.


### Example


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

No authorization required

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

No authorization required

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
on the target object — and emits a single audit entry
.


### Example


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

No authorization required

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

# **update_label_definition**
> LabelDefinition update_label_definition(id, label_definition_update_request)

Update mutable fields on a Label Definition.

Updates the Label Definition identified by `{id}`. Immutable
Definitions reject value-schema changes with 409
`immutable-violation`.


### Example


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

No authorization required

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
**409** | Immutable Definition rejected a value-schema change .  |  -  |
**422** | Aggregate invariant violated. |  -  |
**500** | Internal server error. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

