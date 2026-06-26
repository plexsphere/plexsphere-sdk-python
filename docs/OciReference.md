# OciReference

Reference naming where a catalog source's OCI bundle lives: a registry host, a repository path, and exactly one of a mutable `tag` or an immutable `digest`. Supplying both or neither is rejected with `400 invalid_oci_reference`. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**registry** | **str** | Registry host with an optional port, e.g. &#x60;ghcr.io&#x60;. | 
**repository** | **str** | Slash-separated repository path, e.g. &#x60;plexsphere/catalog&#x60;. | 
**tag** | **str** | Mutable tag, e.g. &#x60;v1.2.3&#x60;. Set exactly one of &#x60;tag&#x60; or &#x60;digest&#x60;. A &#x60;track-tag&#x60; tracking policy requires a tag.  | [optional] 
**digest** | **str** | Immutable &#x60;sha256:&lt;64 hex&gt;&#x60; content digest. Set exactly one of &#x60;tag&#x60; or &#x60;digest&#x60;.  | [optional] 

## Example

```python
from plexsphere.models.oci_reference import OciReference

# TODO update the JSON string below
json = "{}"
# create an instance of OciReference from a JSON string
oci_reference_instance = OciReference.from_json(json)
# print the JSON string representation of the object
print(OciReference.to_json())

# convert the object into a dict
oci_reference_dict = oci_reference_instance.to_dict()
# create an instance of OciReference from a dict
oci_reference_from_dict = OciReference.from_dict(oci_reference_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


