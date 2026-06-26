# BackupExclusion

One entry of what the platform deliberately does not back up, with the rationale and the recovery path that covers the gap. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**name** | **str** | What is not backed up. | 
**rationale** | **str** | Why it is not backed up. | 
**recovery_path** | **str** | How the gap is recovered without a backup. | 

## Example

```python
from plexsphere.models.backup_exclusion import BackupExclusion

# TODO update the JSON string below
json = "{}"
# create an instance of BackupExclusion from a JSON string
backup_exclusion_instance = BackupExclusion.from_json(json)
# print the JSON string representation of the object
print(BackupExclusion.to_json())

# convert the object into a dict
backup_exclusion_dict = backup_exclusion_instance.to_dict()
# create an instance of BackupExclusion from a dict
backup_exclusion_from_dict = BackupExclusion.from_dict(backup_exclusion_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


