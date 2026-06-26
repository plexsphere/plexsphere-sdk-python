# TrackingPolicy

Whether and how often a catalog source is re-resolved. A `pinned` policy never re-resolves. A `track-tag` policy re-resolves the source's tag on a fixed interval and therefore requires the `oci_reference` to be tag-pinned. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**mode** | **str** | Discriminator. &#x60;track-tag&#x60; requires a positive &#x60;interval_seconds&#x60; and a tag-pinned &#x60;oci_reference&#x60;; &#x60;pinned&#x60; must carry no interval.  | 
**interval_seconds** | **int** | Re-resolution interval in seconds. Required and positive for &#x60;track-tag&#x60;; absent for &#x60;pinned&#x60;.  | [optional] 

## Example

```python
from plexsphere.models.tracking_policy import TrackingPolicy

# TODO update the JSON string below
json = "{}"
# create an instance of TrackingPolicy from a JSON string
tracking_policy_instance = TrackingPolicy.from_json(json)
# print the JSON string representation of the object
print(TrackingPolicy.to_json())

# convert the object into a dict
tracking_policy_dict = tracking_policy_instance.to_dict()
# create an instance of TrackingPolicy from a dict
tracking_policy_from_dict = TrackingPolicy.from_dict(tracking_policy_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


