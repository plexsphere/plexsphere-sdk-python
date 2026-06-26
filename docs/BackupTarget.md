# BackupTarget

One recovery-objective row: the recovery-point objective (RPO) and recovery-time objective (RTO) for a plane. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**plane** | **str** | The plane the objectives apply to. | 
**rpo** | **str** | Recovery-point objective for the plane. | 
**rto** | **str** | Recovery-time objective for the plane. | 

## Example

```python
from plexsphere.models.backup_target import BackupTarget

# TODO update the JSON string below
json = "{}"
# create an instance of BackupTarget from a JSON string
backup_target_instance = BackupTarget.from_json(json)
# print the JSON string representation of the object
print(BackupTarget.to_json())

# convert the object into a dict
backup_target_dict = backup_target_instance.to_dict()
# create an instance of BackupTarget from a dict
backup_target_from_dict = BackupTarget.from_dict(backup_target_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


