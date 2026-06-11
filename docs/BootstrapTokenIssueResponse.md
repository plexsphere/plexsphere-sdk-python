# BootstrapTokenIssueResponse

Response body for POST /v1/projects/{project_id}/bootstrap-tokens. The plaintext `token` is returned exactly once — subsequent reads (GetBootstrapTokenMetadata, ListBootstrapTokens) NEVER include it because the persistence layer stores only the Argon2id hash. Operators MUST capture the plaintext from this response and hand it to the redeeming Node/Bridge out-of-band . 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**token_id** | **UUID** | BootstrapToken identifier (UUIDv7). | 
**token** | **str** | Plaintext bearer string in the documented &#x60;psb_&lt;env&gt;_&lt;projectId&gt;_&lt;kind&gt;_&lt;random&gt;&#x60; format (matches &#x60;^psb_[a-z]+_[a-z2-7]+_(node|bridge)_[a-z2-7]{20,}$&#x60;). **Shown exactly once** — there is no API surface that ever returns the plaintext again. The Spectral rule policed by task 4.2 keys off the &#x60;x-plexsphere-once: true&#x60; extension on this property.  | 
**issued_at** | **datetime** | Issuance timestamp (UTC). | 
**expires_at** | **datetime** | Absolute expiry derived from &#x60;issued_at + ttl_seconds&#x60;. After this point the token is no longer redeemable.  | 

## Example

```python
from plexsphere.models.bootstrap_token_issue_response import BootstrapTokenIssueResponse

# TODO update the JSON string below
json = "{}"
# create an instance of BootstrapTokenIssueResponse from a JSON string
bootstrap_token_issue_response_instance = BootstrapTokenIssueResponse.from_json(json)
# print the JSON string representation of the object
print(BootstrapTokenIssueResponse.to_json())

# convert the object into a dict
bootstrap_token_issue_response_dict = bootstrap_token_issue_response_instance.to_dict()
# create an instance of BootstrapTokenIssueResponse from a dict
bootstrap_token_issue_response_from_dict = BootstrapTokenIssueResponse.from_dict(bootstrap_token_issue_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


