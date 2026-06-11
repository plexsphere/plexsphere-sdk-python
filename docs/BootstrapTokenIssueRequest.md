# BootstrapTokenIssueRequest

Body for POST /v1/projects/{project_id}/bootstrap-tokens. Field set mirrors the BootstrapToken aggregate's issuance invariants. The aggregate rejects unknown `kind` values, malformed `env_prefix`, and TTL values outside `[300, 86400]` seconds with a 400 Problem.  DECISION: the `issued_by_user_id` field is intentionally absent. The handler binds the persisted `issued_by_user_id` to the authenticated principal id from the request context, NOT to a client-supplied value. Earlier revisions accepted the field from the request body, which let a privileged caller forge an arbitrary issuer attribution into the audit trail. Removing the field from the wire schema makes that spoof impossible at the schema-validation boundary. 

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**kind** | **str** | Discriminates the redemption surface — &#x60;node&#x60; tokens are redeemed at the Node bootstrap endpoint, &#x60;bridge&#x60; tokens at the Bridge bootstrap endpoint.  | 
**env_prefix** | **str** | Lowercase ASCII letter run encoded as the environment segment of the plaintext. Same regex enforced by the aggregate and the parser, so the surface and the storage layer agree on the allowed alphabet.  | 
**ttl_seconds** | **int** | Lifetime of the BootstrapToken in seconds. Bounded by the aggregate&#39;s &#x60;[MinTTL&#x3D;5m, MaxTTL&#x3D;24h]&#x60; window so a forgotten token cannot become a long-lived bearer credential .  | 

## Example

```python
from plexsphere.models.bootstrap_token_issue_request import BootstrapTokenIssueRequest

# TODO update the JSON string below
json = "{}"
# create an instance of BootstrapTokenIssueRequest from a JSON string
bootstrap_token_issue_request_instance = BootstrapTokenIssueRequest.from_json(json)
# print the JSON string representation of the object
print(BootstrapTokenIssueRequest.to_json())

# convert the object into a dict
bootstrap_token_issue_request_dict = bootstrap_token_issue_request_instance.to_dict()
# create an instance of BootstrapTokenIssueRequest from a dict
bootstrap_token_issue_request_from_dict = BootstrapTokenIssueRequest.from_dict(bootstrap_token_issue_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


