# ServiceFederationKind

Credential shape a service identity authenticates with. `oidc_cc` is an OIDC client-credentials access token from the Domain's IdPBinding; `spiffe_svid` is a JWT-SVID from a SPIFFE workload API; `api_token` is a plexsphere-issued bearer bound to the identity. The pin is enforced at sign-in: a credential of a different kind is rejected without inspecting its payload. The three values mirror the plexsphere.service_identities.federation_kind CHECK constraint. 

## Enum

* `OIDC_CC` (value: `'oidc_cc'`)

* `SPIFFE_SVID` (value: `'spiffe_svid'`)

* `API_TOKEN` (value: `'api_token'`)

[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


