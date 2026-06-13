# ManagedPushAttachRequest

Body for `PUT /v1/domains/{domainId}/managed-push`. Attaches or replaces the Domain's managed-push target. The kubeconfig plaintext carried here is base64-encoded, sealed at rest, and is NEVER echoed back over the API — the read projection exposes only a hex fingerprint of the plaintext. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**kubeconfig_b64** | **bytes** | Base64-encoded kubeconfig for the managed-push cluster. The decoded plaintext is validated and sealed at rest; it is never echoed back in any response. Only embedded, in-band credentials are accepted (a token, or &#x60;client-certificate-data&#x60; / &#x60;client-key-data&#x60;, with &#x60;certificate-authority-data&#x60;); a kubeconfig carrying an &#x60;exec&#x60; or &#x60;auth-provider&#x60; plugin, a file-path credential (&#x60;tokenFile&#x60;, &#x60;client-certificate&#x60;, &#x60;client-key&#x60;), a &#x60;certificate-authority&#x60; file path, or a &#x60;proxy-url&#x60; is rejected with &#x60;kubeconfig_invalid&#x60;.  | 
**enabled** | **bool** | Whether managed pushes are admitted against this target. A disabled target rejects &#x60;PushManagedHook&#x60; with &#x60;managed_push_disabled&#x60;.  | 

## Example

```python
from plexsphere.models.managed_push_attach_request import ManagedPushAttachRequest

# TODO update the JSON string below
json = "{}"
# create an instance of ManagedPushAttachRequest from a JSON string
managed_push_attach_request_instance = ManagedPushAttachRequest.from_json(json)
# print the JSON string representation of the object
print(ManagedPushAttachRequest.to_json())

# convert the object into a dict
managed_push_attach_request_dict = managed_push_attach_request_instance.to_dict()
# create an instance of ManagedPushAttachRequest from a dict
managed_push_attach_request_from_dict = ManagedPushAttachRequest.from_dict(managed_push_attach_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


