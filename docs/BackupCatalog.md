# BackupCatalog

The platform-scoped backup coverage catalog: the per-store coverage table, the recovery objectives per plane, and the explicit exclusions. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**entries** | [**List[BackupCatalogEntry]**](BackupCatalogEntry.md) | The per-store coverage table. | 
**targets** | [**List[BackupTarget]**](BackupTarget.md) | The recovery objectives per plane. | 
**exclusions** | [**List[BackupExclusion]**](BackupExclusion.md) | What the platform deliberately does not back up. | 

## Example

```python
from plexsphere.models.backup_catalog import BackupCatalog

# TODO update the JSON string below
json = "{}"
# create an instance of BackupCatalog from a JSON string
backup_catalog_instance = BackupCatalog.from_json(json)
# print the JSON string representation of the object
print(BackupCatalog.to_json())

# convert the object into a dict
backup_catalog_dict = backup_catalog_instance.to_dict()
# create an instance of BackupCatalog from a dict
backup_catalog_from_dict = BackupCatalog.from_dict(backup_catalog_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


