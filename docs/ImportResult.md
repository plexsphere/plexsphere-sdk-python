# ImportResult

The per-Blueprint outcomes of an import, returned by `POST /v1/blueprint-catalogs/{id}/import`. Drift and conflict are per-Blueprint, not batch-fatal, so the `outcomes` array reports every requested slug. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**outcomes** | [**List[ImportOutcome]**](ImportOutcome.md) | One entry per requested Blueprint slug. | 

## Example

```python
from plexsphere.models.import_result import ImportResult

# TODO update the JSON string below
json = "{}"
# create an instance of ImportResult from a JSON string
import_result_instance = ImportResult.from_json(json)
# print the JSON string representation of the object
print(ImportResult.to_json())

# convert the object into a dict
import_result_dict = import_result_instance.to_dict()
# create an instance of ImportResult from a dict
import_result_from_dict = ImportResult.from_dict(import_result_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


