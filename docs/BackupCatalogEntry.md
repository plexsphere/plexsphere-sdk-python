# BackupCatalogEntry

One row of the backup coverage table: a store, how it is backed up, how often, how long it is retained, its restore priority, and an optional restore-sequence step it maps to. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**store** | **str** | The store the row describes. | 
**method** | **str** | How the store is backed up. | 
**cadence** | **str** | How often the store is backed up. | 
**retention** | **str** | How long the store&#39;s backups are retained. | 
**priority** | [**BackupPriority**](BackupPriority.md) |  | 
**restore_step** | **int** | The restore-plan step order this store is restored in, or &#x60;0&#x60; when the store is not part of the linear restore sequence.  | 
**notes** | **str** | Free-form notes about backing up or restoring the store. | 

## Example

```python
from plexsphere.models.backup_catalog_entry import BackupCatalogEntry

# TODO update the JSON string below
json = "{}"
# create an instance of BackupCatalogEntry from a JSON string
backup_catalog_entry_instance = BackupCatalogEntry.from_json(json)
# print the JSON string representation of the object
print(BackupCatalogEntry.to_json())

# convert the object into a dict
backup_catalog_entry_dict = backup_catalog_entry_instance.to_dict()
# create an instance of BackupCatalogEntry from a dict
backup_catalog_entry_from_dict = BackupCatalogEntry.from_dict(backup_catalog_entry_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


