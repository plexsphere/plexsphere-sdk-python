# ResourceCreateRequest

Body for `POST /v1/projects/{project_id}/resources`. The `origin` field discriminates the two creation flows:    * `origin=adopted` requires `kind`; `external_ref` is     optional. The handler persists the Resource and returns     `201` with the hydrated `ResourceResponse`.   * `origin=provisioned` requires `kind`,     `blueprint_version_id`, and `cloud_credential_id`; the     optional `parameters` object carries the typed values     validated against the BlueprintVersion's     `parameter_schema`. The handler creates the Resource,     mints a Pending `ProvisionedResource` via the broker, and     returns `202` with a `Location` header.  A body with `origin` absent or outside `{adopted, provisioned}` is rejected with `400 invalid_resource_origin`. A body that mixes flow-specific fields with the wrong `origin` (e.g. a `blueprint_version_id` on `origin=adopted`) is rejected with `400 invalid_body`. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**origin** | [**ResourceOrigin**](ResourceOrigin.md) |  | 
**kind** | **str** | Resource kind discriminator. Trimmed and bounded by the tenancy aggregate&#39;s &#x60;resourceKindMaxLen&#x60;.  | 
**external_ref** | **str** | Optional external-system reference for the adopted flow (URN, ARN, vendor-opaque id). Trimmed; whitespace-only is rejected. Omit on the provisioned flow.  | [optional] 
**blueprint_version_id** | **UUID** | BlueprintVersion the broker should render. REQUIRED on &#x60;origin&#x3D;provisioned&#x60;; omit on &#x60;origin&#x3D;adopted&#x60;. A missing version row surfaces as &#x60;422&#x60; with &#x60;code: blueprint_not_found&#x60;.  | [optional] 
**cloud_credential_id** | **UUID** | Cloud Credential the broker uses to provision the substrate. REQUIRED on &#x60;origin&#x3D;provisioned&#x60;; omit on &#x60;origin&#x3D;adopted&#x60;. A missing credential row surfaces as &#x60;422&#x60; with &#x60;code: cloud_credential_not_found&#x60;.  | [optional] 
**parameters** | **Dict[str, object]** | Typed parameter values for &#x60;origin&#x3D;provisioned&#x60;, validated against the BlueprintVersion&#39;s typed &#x60;parameter_schema&#x60;. Omit on &#x60;origin&#x3D;adopted&#x60;. The wire envelope is opaque here; type-shape rejections surface at the handler.  | [optional] 

## Example

```python
from plexsphere.models.resource_create_request import ResourceCreateRequest

# TODO update the JSON string below
json = "{}"
# create an instance of ResourceCreateRequest from a JSON string
resource_create_request_instance = ResourceCreateRequest.from_json(json)
# print the JSON string representation of the object
print(ResourceCreateRequest.to_json())

# convert the object into a dict
resource_create_request_dict = resource_create_request_instance.to_dict()
# create an instance of ResourceCreateRequest from a dict
resource_create_request_from_dict = ResourceCreateRequest.from_dict(resource_create_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


