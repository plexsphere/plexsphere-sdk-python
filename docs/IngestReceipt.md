# IngestReceipt

Body for the `202 Accepted` response of the three observability ingest operations (`POST /v1/nodes/{id}/metrics`, `.../logs`, and `.../audit`). Carries the server-side acceptance timestamp and the echoed count of records buffered, so the agent can reconcile the batch with its local replay queue. Mirrors the shape of `IntegrityViolationsResponse`. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**accepted_at** | **datetime** | Server-side timestamp at which the batch was accepted and handed to the ingest buffer.  | 
**records** | **int** | Number of records accepted from the batch. Matches the number of array elements (metrics) or non-blank NDJSON lines (logs / audit) on input.  | 

## Example

```python
from plexsphere.models.ingest_receipt import IngestReceipt

# TODO update the JSON string below
json = "{}"
# create an instance of IngestReceipt from a JSON string
ingest_receipt_instance = IngestReceipt.from_json(json)
# print the JSON string representation of the object
print(IngestReceipt.to_json())

# convert the object into a dict
ingest_receipt_dict = ingest_receipt_instance.to_dict()
# create an instance of IngestReceipt from a dict
ingest_receipt_from_dict = IngestReceipt.from_dict(ingest_receipt_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


