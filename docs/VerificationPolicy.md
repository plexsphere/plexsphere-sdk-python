# VerificationPolicy

How a catalog source's bundle signature is verified before import. A `pinned` policy admits only bundles whose keyless cosign signing identity matches `identity_san` (a regexp over the certificate SAN) and `issuer`. An `unsigned` policy accepts the bundle without signature verification. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**mode** | **str** | Discriminator. &#x60;pinned&#x60; requires &#x60;identity_san&#x60; and &#x60;issuer&#x60;; &#x60;unsigned&#x60; must carry neither.  | 
**identity_san** | **str** | Regexp matched against the signing certificate&#39;s SAN. Required for &#x60;pinned&#x60;; must be absent for &#x60;unsigned&#x60;.  | [optional] 
**issuer** | **str** | Expected OIDC issuer. Required for &#x60;pinned&#x60;; must be absent for &#x60;unsigned&#x60;.  | [optional] 

## Example

```python
from plexsphere.models.verification_policy import VerificationPolicy

# TODO update the JSON string below
json = "{}"
# create an instance of VerificationPolicy from a JSON string
verification_policy_instance = VerificationPolicy.from_json(json)
# print the JSON string representation of the object
print(VerificationPolicy.to_json())

# convert the object into a dict
verification_policy_dict = verification_policy_instance.to_dict()
# create an instance of VerificationPolicy from a dict
verification_policy_from_dict = VerificationPolicy.from_dict(verification_policy_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


