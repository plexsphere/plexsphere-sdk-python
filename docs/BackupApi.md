# plexsphere.BackupApi

All URIs are relative to *http://localhost*

Method | HTTP request | Description
------------- | ------------- | -------------
[**get_backup_catalog**](BackupApi.md#get_backup_catalog) | **GET** /v1/platform/backup/catalog | Return the backup coverage catalog.
[**get_backup_restore_plan**](BackupApi.md#get_backup_restore_plan) | **GET** /v1/platform/backup/restore-plan | Return the ordered disaster-recovery restore plan.


# **get_backup_catalog**
> BackupCatalog get_backup_catalog()

Return the backup coverage catalog.

Returns the platform-scoped, read-only backup coverage catalog: the
per-store coverage table (what is backed up, how, how often, and how
long it is retained), the RPO/RTO targets per plane, and the explicit
exclusions of what the platform deliberately does not back up. The read
is gated by a platform-scope ReBAC check before any data is returned.


### Example


```python
import plexsphere
from plexsphere.models.backup_catalog import BackupCatalog
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
    api_instance = plexsphere.BackupApi(api_client)

    try:
        # Return the backup coverage catalog.
        api_response = api_instance.get_backup_catalog()
        print("The response of BackupApi->get_backup_catalog:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling BackupApi->get_backup_catalog: %s\n" % e)
```



### Parameters

This endpoint does not need any parameter.

### Return type

[**BackupCatalog**](BackupCatalog.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | The backup coverage catalog. |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the platform-scope ReBAC relation required to read the backup catalog.  |  -  |
**500** | Internal server error. |  -  |
**501** | The backup surface is not yet provisioned in this build, so log scrapers can alert on the deferred-wiring state.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **get_backup_restore_plan**
> RestorePlan get_backup_restore_plan()

Return the ordered disaster-recovery restore plan.

Returns the platform-scoped, read-only restore plan: the ordered
sequence of steps operators follow during disaster recovery, each
naming the store to restore, the action to take, and how to verify the
step succeeded. The read is gated by a platform-scope ReBAC check
before any data is returned.


### Example


```python
import plexsphere
from plexsphere.models.restore_plan import RestorePlan
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
    api_instance = plexsphere.BackupApi(api_client)

    try:
        # Return the ordered disaster-recovery restore plan.
        api_response = api_instance.get_backup_restore_plan()
        print("The response of BackupApi->get_backup_restore_plan:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling BackupApi->get_backup_restore_plan: %s\n" % e)
```



### Parameters

This endpoint does not need any parameter.

### Return type

[**RestorePlan**](RestorePlan.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | The ordered restore plan. |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the platform-scope ReBAC relation required to read the restore plan.  |  -  |
**500** | Internal server error. |  -  |
**501** | The backup surface is not yet provisioned in this build, so log scrapers can alert on the deferred-wiring state.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

