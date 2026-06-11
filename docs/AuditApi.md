# plexsphere.AuditApi

All URIs are relative to *http://localhost*

Method | HTTP request | Description
------------- | ------------- | -------------
[**erase_identity_from_audit**](AuditApi.md#erase_identity_from_audit) | **POST** /v1/domains/{domainId}/audit/erase-identity | Erase an identity&#39;s PII mapping from the per-Domain audit log.
[**erase_identity_from_platform_audit**](AuditApi.md#erase_identity_from_platform_audit) | **POST** /v1/platform/audit/erase-identity | Erase an identity&#39;s PII mapping from the platform audit log.
[**get_audit_entry**](AuditApi.md#get_audit_entry) | **GET** /v1/domains/{domainId}/audit/entries/{seq} | Read a single audit entry with its hash-chain proof.
[**get_platform_audit_entry**](AuditApi.md#get_platform_audit_entry) | **GET** /v1/platform/audit/entries/{seq} | Read a single platform audit entry with its hash-chain proof.
[**list_audit_entries**](AuditApi.md#list_audit_entries) | **GET** /v1/domains/{domainId}/audit/entries | List entries on the per-Domain audit chain.
[**list_platform_audit_entries**](AuditApi.md#list_platform_audit_entries) | **GET** /v1/platform/audit/entries | List entries on the platform-residency audit chain.
[**verify_audit_chain**](AuditApi.md#verify_audit_chain) | **POST** /v1/domains/{domainId}/audit/verify | Verify the per-Domain hash chain or a segment of it.
[**verify_platform_audit_chain**](AuditApi.md#verify_platform_audit_chain) | **POST** /v1/platform/audit/verify | Verify the platform-residency hash chain or a segment of it.


# **erase_identity_from_audit**
> AuditEraseIdentityResponse erase_identity_from_audit(domain_id, audit_erase_identity_request)

Erase an identity's PII mapping from the per-Domain audit log.

Right-to-erasure entry point. Drops the `audit_subject_pii`
row for the per-Domain pseudonym derived from `identity_id`,
then appends an `audit.erase-identity` self-audit entry to
the same chain so the erasure event is itself auditable
. The hash chain remains verifiable: rows
reference the pseudonym, and the pseudonym is preserved —
only the plaintext mapping is removed.

The endpoint is IDEMPOTENT on `subject_pseudonym`. A second
call after a successful erasure is a no-op on the
`audit_subject_pii` table (the row is already gone) and may
emit a second self-audit entry; callers MAY safely retry
after a partial network failure. The 202 status reflects the
idempotent contract: a successful erasure has been recorded,
even if the underlying PII row was already absent.


### Example


```python
import plexsphere
from plexsphere.models.audit_erase_identity_request import AuditEraseIdentityRequest
from plexsphere.models.audit_erase_identity_response import AuditEraseIdentityResponse
from plexsphere.rest import ApiException
from pprint import pprint

# Defining the host is optional and defaults to http://localhost
# See configuration.py for a list of all supported configuration parameters.
configuration = plexsphere.Configuration(
    host = "http://localhost"
)


# Enter a context with an instance of the API client
with plexsphere.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = plexsphere.AuditApi(api_client)
    domain_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Domain. The Platform Audit Log is per-Domain by residency contract — cross-Domain decisions land in EACH affected Domain's chain, never on a shared system chain . Every audit endpoint requires the Domain identifier in the path. 
    audit_erase_identity_request = {"identity_id":"0190a8b8-a0c0-7a0a-8a0a-a0a0a0a0a0a0"} # AuditEraseIdentityRequest | 

    try:
        # Erase an identity's PII mapping from the per-Domain audit log.
        api_response = api_instance.erase_identity_from_audit(domain_id, audit_erase_identity_request)
        print("The response of AuditApi->erase_identity_from_audit:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling AuditApi->erase_identity_from_audit: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **domain_id** | **UUID**| Owning Domain. The Platform Audit Log is per-Domain by residency contract — cross-Domain decisions land in EACH affected Domain&#39;s chain, never on a shared system chain . Every audit endpoint requires the Domain identifier in the path.  | 
 **audit_erase_identity_request** | [**AuditEraseIdentityRequest**](AuditEraseIdentityRequest.md)|  | 

### Return type

[**AuditEraseIdentityResponse**](AuditEraseIdentityResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**202** | Erasure recorded (idempotent on &#x60;subject_pseudonym&#x60;). The response carries the per-Domain pseudonym derived from &#x60;identity_id&#x60; so operators can correlate the self-audit entry without re-deriving it.  |  -  |
**400** | Invalid erase request (malformed &#x60;identity_id&#x60;, empty body).  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the &#x60;auditor&#x60; ReBAC relation on the addressed Domain.  |  -  |
**404** | Domain id not found. Surfaced only after the authorisation gate has passed.  |  -  |
**500** | Internal error: persistence-layer failure, authz-check crash, or pseudonym derivation failure on the response leg.  |  -  |
**501** | Audit erase service / authorizer / pepper resolver not yet provisioned in this build. Body is a Problem with &#x60;code: audit_not_provisioned&#x60;.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **erase_identity_from_platform_audit**
> AuditEraseIdentityResponse erase_identity_from_platform_audit(audit_erase_identity_request)

Erase an identity's PII mapping from the platform audit log.

Right-to-erasure entry point. Drops the `audit_subject_pii`
row for the platform-residency pseudonym derived from
`identity_id`, then appends an `audit.erase-identity`
self-audit entry to the platform-residency chain so the
erasure event is itself auditable. The hash chain remains
verifiable: rows reference the pseudonym, and the pseudonym is
preserved — only the plaintext mapping is removed.

The endpoint is IDEMPOTENT on `subject_pseudonym`. A second
call after a successful erasure is a no-op on the
`audit_subject_pii` table (the row is already gone) and may
emit a second self-audit entry; callers MAY safely retry
after a partial network failure. The 202 status reflects the
idempotent contract: a successful erasure has been recorded,
even if the underlying PII row was already absent.


### Example


```python
import plexsphere
from plexsphere.models.audit_erase_identity_request import AuditEraseIdentityRequest
from plexsphere.models.audit_erase_identity_response import AuditEraseIdentityResponse
from plexsphere.rest import ApiException
from pprint import pprint

# Defining the host is optional and defaults to http://localhost
# See configuration.py for a list of all supported configuration parameters.
configuration = plexsphere.Configuration(
    host = "http://localhost"
)


# Enter a context with an instance of the API client
with plexsphere.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = plexsphere.AuditApi(api_client)
    audit_erase_identity_request = {identity_id=0190a8b8-a0c0-7a0a-8a0a-a0a0a0a0a0a0} # AuditEraseIdentityRequest | 

    try:
        # Erase an identity's PII mapping from the platform audit log.
        api_response = api_instance.erase_identity_from_platform_audit(audit_erase_identity_request)
        print("The response of AuditApi->erase_identity_from_platform_audit:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling AuditApi->erase_identity_from_platform_audit: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **audit_erase_identity_request** | [**AuditEraseIdentityRequest**](AuditEraseIdentityRequest.md)|  | 

### Return type

[**AuditEraseIdentityResponse**](AuditEraseIdentityResponse.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**202** | Erasure recorded (idempotent on &#x60;subject_pseudonym&#x60;). The response carries the platform-residency pseudonym derived from &#x60;identity_id&#x60; so operators can correlate the self-audit entry without re-deriving it.  |  -  |
**400** | Invalid erase request (malformed &#x60;identity_id&#x60;, empty body).  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the &#x60;manage&#x60; ReBAC permission on the platform object.  |  -  |
**500** | Internal error: persistence-layer failure, authz-check crash, or pseudonym derivation failure on the response leg.  |  -  |
**501** | Audit erase service / authorizer / pepper resolver not yet provisioned in this build. Body is a Problem with &#x60;code: audit_not_provisioned&#x60;.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **get_audit_entry**
> AuditEntryProof get_audit_entry(domain_id, seq)

Read a single audit entry with its hash-chain proof.

Returns the audit entry at `seq` on the addressed Domain's
chain, together with the forensic proof needed to recompute
and verify the entry's hash off-line. The proof carries `prev_hash`, `entry_hash`, and
the canonical bytes the chain hashed — `sha256(prev_hash ||
sha256(canonical_bytes))` is the computed `entry_hash`, and
comparing it against the stored value pins divergence at the
offending row.

Cross-Domain reads are not allowed: a `seq` that exists on a
DIFFERENT Domain's chain surfaces as 404 with the same shape
as a truly-unknown `seq` so the endpoint cannot be used as a
cross-Domain enumeration oracle. The
404-after-403 ordering matches the ListAuditEntries gate.

UNAUTHORISED READS COLLAPSE ONTO 404. Unlike ListAuditEntries
and VerifyAuditChain, this surface deliberately maps a
missing `auditor` relation to 404 (not 403) so the response
is byte-indistinguishable from "row does not exist on this
Domain". The seq id is per-Domain monotonic from 1 — a 403
would otherwise leak chain length and let an attacker probe
for the existence of arbitrary rows across Domains. The
same probe-oracle defence applies on cross-tenant reads
.


### Example


```python
import plexsphere
from plexsphere.models.audit_entry_proof import AuditEntryProof
from plexsphere.rest import ApiException
from pprint import pprint

# Defining the host is optional and defaults to http://localhost
# See configuration.py for a list of all supported configuration parameters.
configuration = plexsphere.Configuration(
    host = "http://localhost"
)


# Enter a context with an instance of the API client
with plexsphere.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = plexsphere.AuditApi(api_client)
    domain_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Domain. The Platform Audit Log is per-Domain by residency contract — cross-Domain decisions land in EACH affected Domain's chain, never on a shared system chain . Every audit endpoint requires the Domain identifier in the path. 
    seq = 56 # int | Per-Domain monotonic sequence number assigned at append time. Sequences start at 1 (the genesis row) and never reset. 

    try:
        # Read a single audit entry with its hash-chain proof.
        api_response = api_instance.get_audit_entry(domain_id, seq)
        print("The response of AuditApi->get_audit_entry:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling AuditApi->get_audit_entry: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **domain_id** | **UUID**| Owning Domain. The Platform Audit Log is per-Domain by residency contract — cross-Domain decisions land in EACH affected Domain&#39;s chain, never on a shared system chain . Every audit endpoint requires the Domain identifier in the path.  | 
 **seq** | **int**| Per-Domain monotonic sequence number assigned at append time. Sequences start at 1 (the genesis row) and never reset.  | 

### Return type

[**AuditEntryProof**](AuditEntryProof.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Audit entry plus the canonical-bytes proof needed to recompute its hash.  |  -  |
**400** | Invalid &#x60;seq&#x60; path parameter (e.g. &#x60;seq &lt; 1&#x60;) or other client-side input fault.  |  -  |
**401** | Caller is not authenticated. |  -  |
**404** | No entry at this &#x60;(domainId, seq)&#x60;. Returned for unknown sequences, for sequences that exist on a DIFFERENT Domain&#39;s chain (cross-Domain enumeration oracle defence), AND for callers who lack the &#x60;auditor&#x60; ReBAC relation on the addressed Domain — the unauthorised-read leg deliberately collapses onto 404 so the response is byte-indistinguishable from \&quot;row does not exist\&quot;. See the operation description for the full probe-oracle rationale .  |  -  |
**500** | Internal error: persistence-layer failure, authz-check crash, or canonical-encode crash on the read path .  |  -  |
**501** | Audit chain store / authorizer not yet provisioned in this build. Body is a Problem with &#x60;code: audit_not_provisioned&#x60;.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **get_platform_audit_entry**
> AuditEntryProof get_platform_audit_entry(seq)

Read a single platform audit entry with its hash-chain proof.

Returns the audit entry at `seq` on the platform-residency
chain, together with the forensic proof needed to recompute
and verify the entry's hash off-line. The proof carries
`prev_hash`, `entry_hash`, and the canonical bytes the chain
hashed — `sha256(prev_hash || sha256(canonical_bytes))` is
the computed `entry_hash`, and comparing it against the
stored value pins divergence at the offending row.

UNAUTHORISED READS COLLAPSE ONTO 404. Unlike
ListPlatformAuditEntries and VerifyPlatformAuditChain, this
surface deliberately maps a missing `auditor` relation to 404
(not 403) so the response is byte-indistinguishable from "row
does not exist". The seq id is monotonic from 1 — a 403 would
otherwise leak chain length and let an attacker probe for the
existence of arbitrary rows.


### Example


```python
import plexsphere
from plexsphere.models.audit_entry_proof import AuditEntryProof
from plexsphere.rest import ApiException
from pprint import pprint

# Defining the host is optional and defaults to http://localhost
# See configuration.py for a list of all supported configuration parameters.
configuration = plexsphere.Configuration(
    host = "http://localhost"
)


# Enter a context with an instance of the API client
with plexsphere.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = plexsphere.AuditApi(api_client)
    seq = 56 # int | Monotonic sequence number assigned at append time. Sequences start at 1 (the genesis row) and never reset. 

    try:
        # Read a single platform audit entry with its hash-chain proof.
        api_response = api_instance.get_platform_audit_entry(seq)
        print("The response of AuditApi->get_platform_audit_entry:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling AuditApi->get_platform_audit_entry: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **seq** | **int**| Monotonic sequence number assigned at append time. Sequences start at 1 (the genesis row) and never reset.  | 

### Return type

[**AuditEntryProof**](AuditEntryProof.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Audit entry plus the canonical-bytes proof needed to recompute its hash.  |  -  |
**400** | Invalid &#x60;seq&#x60; path parameter (e.g. &#x60;seq &lt; 1&#x60;) or other client-side input fault.  |  -  |
**401** | Caller is not authenticated. |  -  |
**404** | No entry at this &#x60;seq&#x60;. Returned for unknown sequences AND for callers who lack the &#x60;auditor&#x60; ReBAC relation on the platform object — the unauthorised-read leg deliberately collapses onto 404 so the response is byte-indistinguishable from \&quot;row does not exist\&quot;. See the operation description for the full probe-oracle rationale .  |  -  |
**500** | Internal error: persistence-layer failure, authz-check crash, or canonical-encode crash on the read path .  |  -  |
**501** | Audit chain store / authorizer not yet provisioned in this build. Body is a Problem with &#x60;code: audit_not_provisioned&#x60;.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **list_audit_entries**
> AuditEntryList list_audit_entries(domain_id, cursor=cursor, limit=limit, subject=subject, relation=relation, object_type=object_type, object_id=object_id, reason=reason, var_from=var_from, to=to, correlation_id=correlation_id)

List entries on the per-Domain audit chain.

Returns a cursor-paginated page of audit chain rows for the
addressed Domain. Every read is
gated by the `auditor` ReBAC relation on the addressed Domain;
unauthenticated callers receive 401, authenticated callers
without the relation receive 403, and an unknown Domain id
surfaces as 404 only after the authorisation gate has passed
so the endpoint cannot be used as a Domain-id oracle.

Pagination is opaque-cursor based: the `next_cursor` returned
in the response body is the value to pass back as `cursor` on
the next call. Cursors are HMAC-signed and bound to the
addressed Domain — replaying a cursor minted for a different
Domain surfaces as a 400 with `code: cursor_invalid`. When the page is shorter than the requested `limit`
the response omits `next_cursor` so callers stop on a single
condition.

Filter semantics: every filter is optional and is AND-composed
in the persistence layer. `subject` is the per-Domain
pseudonym hex (the chain-input form, never plaintext — plaintext lives in `audit_subject_pii` and is purged on right-to-erasure). `from`/`to` bracket the
`occurred_at` server-side timestamp. `correlation_id` matches
the inbound request correlation id propagated from the
transport layer.


### Example


```python
import plexsphere
from plexsphere.models.audit_entry_list import AuditEntryList
from plexsphere.models.audit_reason import AuditReason
from plexsphere.rest import ApiException
from pprint import pprint

# Defining the host is optional and defaults to http://localhost
# See configuration.py for a list of all supported configuration parameters.
configuration = plexsphere.Configuration(
    host = "http://localhost"
)


# Enter a context with an instance of the API client
with plexsphere.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = plexsphere.AuditApi(api_client)
    domain_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Domain. The Platform Audit Log is per-Domain by residency contract — cross-Domain decisions land in EACH affected Domain's chain, never on a shared system chain . Every audit endpoint requires the Domain identifier in the path. 
    cursor = 'cursor_example' # str | Opaque pagination cursor — value of `next_cursor` from the previous page, or unset to start at the head of the chain .  (optional)
    limit = 50 # int | Maximum number of entries to return on this page. Clamped server-side at 200 to protect the read replica from a single auditor issuing an unbounded LIMIT.  (optional) (default to 50)
    subject = 'subject_example' # str | Filter by subject pseudonym (64 lowercase hex characters). The query service never accepts plaintext subject ids; pseudonymisation is the caller's responsibility and happens at the sink boundary.  (optional)
    relation = 'relation_example' # str | SpiceDB relation label to filter on. (optional)
    object_type = 'object_type_example' # str | SpiceDB object type to filter on (e.g. `domain`, `node`). (optional)
    object_id = 'object_id_example' # str | Opaque object identifier within `object_type`. (optional)
    reason = plexsphere.AuditReason() # AuditReason | Filter by ReBAC decision reason.  (optional)
    var_from = '2013-10-20T19:20:30+01:00' # datetime | Inclusive lower bound on `occurred_at` (RFC 3339). (optional)
    to = '2013-10-20T19:20:30+01:00' # datetime | Inclusive upper bound on `occurred_at` (RFC 3339). (optional)
    correlation_id = 'correlation_id_example' # str | Filter by inbound request correlation id. (optional)

    try:
        # List entries on the per-Domain audit chain.
        api_response = api_instance.list_audit_entries(domain_id, cursor=cursor, limit=limit, subject=subject, relation=relation, object_type=object_type, object_id=object_id, reason=reason, var_from=var_from, to=to, correlation_id=correlation_id)
        print("The response of AuditApi->list_audit_entries:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling AuditApi->list_audit_entries: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **domain_id** | **UUID**| Owning Domain. The Platform Audit Log is per-Domain by residency contract — cross-Domain decisions land in EACH affected Domain&#39;s chain, never on a shared system chain . Every audit endpoint requires the Domain identifier in the path.  | 
 **cursor** | **str**| Opaque pagination cursor — value of &#x60;next_cursor&#x60; from the previous page, or unset to start at the head of the chain .  | [optional] 
 **limit** | **int**| Maximum number of entries to return on this page. Clamped server-side at 200 to protect the read replica from a single auditor issuing an unbounded LIMIT.  | [optional] [default to 50]
 **subject** | **str**| Filter by subject pseudonym (64 lowercase hex characters). The query service never accepts plaintext subject ids; pseudonymisation is the caller&#39;s responsibility and happens at the sink boundary.  | [optional] 
 **relation** | **str**| SpiceDB relation label to filter on. | [optional] 
 **object_type** | **str**| SpiceDB object type to filter on (e.g. &#x60;domain&#x60;, &#x60;node&#x60;). | [optional] 
 **object_id** | **str**| Opaque object identifier within &#x60;object_type&#x60;. | [optional] 
 **reason** | [**AuditReason**](.md)| Filter by ReBAC decision reason.  | [optional] 
 **var_from** | **datetime**| Inclusive lower bound on &#x60;occurred_at&#x60; (RFC 3339). | [optional] 
 **to** | **datetime**| Inclusive upper bound on &#x60;occurred_at&#x60; (RFC 3339). | [optional] 
 **correlation_id** | **str**| Filter by inbound request correlation id. | [optional] 

### Return type

[**AuditEntryList**](AuditEntryList.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Page of audit entries for the addressed Domain.  |  -  |
**400** | Invalid query parameter — typically a malformed cursor (&#x60;code: cursor_invalid&#x60;), a non-hex &#x60;subject&#x60;, or a &#x60;to&#x60; earlier than &#x60;from&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the &#x60;auditor&#x60; ReBAC relation on the addressed Domain. Body is a &#x60;PermissionDenied&#x60; problem carrying the denial reason and traversed relation path.  |  -  |
**404** | Domain id not found. Surfaced only after the authorisation gate has passed so the endpoint cannot be used as a Domain-id oracle.  |  -  |
**500** | Internal error: persistence-layer failure, authz-check crash, or canonical-encode crash on the read path. Body is a generic Problem.  |  -  |
**501** | Audit query subsystem not yet provisioned in this build — the composition root has not threaded the audit query service onto the handler. Body is a Problem with &#x60;code: audit_not_provisioned&#x60;.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **list_platform_audit_entries**
> AuditEntryList list_platform_audit_entries(cursor=cursor, limit=limit, subject=subject, relation=relation, object_type=object_type, object_id=object_id, reason=reason, var_from=var_from, to=to, correlation_id=correlation_id)

List entries on the platform-residency audit chain.

Returns a cursor-paginated page of audit chain rows for the
single platform-residency chain — the chain that carries
privileged actions owned by no Domain (Cloud lifecycle,
platform-scope Label definitions, the Invitation
ExpirePending sweep). Every read is gated by the `auditor`
ReBAC relation on the fixed platform object;
unauthenticated callers receive 401 and authenticated
callers without the relation receive 403.

Pagination is opaque-cursor based: the `next_cursor` returned
in the response body is the value to pass back as `cursor` on
the next call. Cursors are HMAC-signed and bound to the
platform-residency chain — replaying a cursor minted against
a different chain surfaces as a 400 with
`code: cursor_invalid`. When the page is shorter than the
requested `limit` the response omits `next_cursor` so callers
stop on a single condition.

Filter semantics: every filter is optional and is AND-composed
in the persistence layer. `subject` is the platform-residency
pseudonym hex (the chain-input form, never plaintext —
plaintext lives in `audit_subject_pii` and is purged on
right-to-erasure). `from`/`to` bracket the `occurred_at`
server-side timestamp. `correlation_id` matches the inbound
request correlation id propagated from the transport layer.


### Example


```python
import plexsphere
from plexsphere.models.audit_entry_list import AuditEntryList
from plexsphere.models.audit_reason import AuditReason
from plexsphere.rest import ApiException
from pprint import pprint

# Defining the host is optional and defaults to http://localhost
# See configuration.py for a list of all supported configuration parameters.
configuration = plexsphere.Configuration(
    host = "http://localhost"
)


# Enter a context with an instance of the API client
with plexsphere.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = plexsphere.AuditApi(api_client)
    cursor = 'cursor_example' # str | Opaque pagination cursor — value of `next_cursor` from the previous page, or unset to start at the head of the chain .  (optional)
    limit = 50 # int | Maximum number of entries to return on this page. Clamped server-side at 200 to protect the read replica from a single auditor issuing an unbounded LIMIT.  (optional) (default to 50)
    subject = 'subject_example' # str | Filter by subject pseudonym (64 lowercase hex characters). The query service never accepts plaintext subject ids; pseudonymisation is the caller's responsibility and happens at the sink boundary.  (optional)
    relation = 'relation_example' # str | SpiceDB relation label to filter on. (optional)
    object_type = 'object_type_example' # str | SpiceDB object type to filter on (e.g. `cloud`, `platform`). (optional)
    object_id = 'object_id_example' # str | Opaque object identifier within `object_type`. (optional)
    reason = plexsphere.AuditReason() # AuditReason | Filter by ReBAC decision reason.  (optional)
    var_from = '2013-10-20T19:20:30+01:00' # datetime | Inclusive lower bound on `occurred_at` (RFC 3339). (optional)
    to = '2013-10-20T19:20:30+01:00' # datetime | Inclusive upper bound on `occurred_at` (RFC 3339). (optional)
    correlation_id = 'correlation_id_example' # str | Filter by inbound request correlation id. (optional)

    try:
        # List entries on the platform-residency audit chain.
        api_response = api_instance.list_platform_audit_entries(cursor=cursor, limit=limit, subject=subject, relation=relation, object_type=object_type, object_id=object_id, reason=reason, var_from=var_from, to=to, correlation_id=correlation_id)
        print("The response of AuditApi->list_platform_audit_entries:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling AuditApi->list_platform_audit_entries: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **cursor** | **str**| Opaque pagination cursor — value of &#x60;next_cursor&#x60; from the previous page, or unset to start at the head of the chain .  | [optional] 
 **limit** | **int**| Maximum number of entries to return on this page. Clamped server-side at 200 to protect the read replica from a single auditor issuing an unbounded LIMIT.  | [optional] [default to 50]
 **subject** | **str**| Filter by subject pseudonym (64 lowercase hex characters). The query service never accepts plaintext subject ids; pseudonymisation is the caller&#39;s responsibility and happens at the sink boundary.  | [optional] 
 **relation** | **str**| SpiceDB relation label to filter on. | [optional] 
 **object_type** | **str**| SpiceDB object type to filter on (e.g. &#x60;cloud&#x60;, &#x60;platform&#x60;). | [optional] 
 **object_id** | **str**| Opaque object identifier within &#x60;object_type&#x60;. | [optional] 
 **reason** | [**AuditReason**](.md)| Filter by ReBAC decision reason.  | [optional] 
 **var_from** | **datetime**| Inclusive lower bound on &#x60;occurred_at&#x60; (RFC 3339). | [optional] 
 **to** | **datetime**| Inclusive upper bound on &#x60;occurred_at&#x60; (RFC 3339). | [optional] 
 **correlation_id** | **str**| Filter by inbound request correlation id. | [optional] 

### Return type

[**AuditEntryList**](AuditEntryList.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Page of audit entries on the platform-residency chain.  |  -  |
**400** | Invalid query parameter — typically a malformed cursor (&#x60;code: cursor_invalid&#x60;), a non-hex &#x60;subject&#x60;, or a &#x60;to&#x60; earlier than &#x60;from&#x60;.  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the &#x60;auditor&#x60; ReBAC relation on the platform object. Body is a &#x60;PermissionDenied&#x60; problem carrying the denial reason and traversed relation path.  |  -  |
**500** | Internal error: persistence-layer failure, authz-check crash, or canonical-encode crash on the read path. Body is a generic Problem.  |  -  |
**501** | Audit query subsystem not yet provisioned in this build — the composition root has not threaded the audit query service onto the handler. Body is a Problem with &#x60;code: audit_not_provisioned&#x60;.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **verify_audit_chain**
> AuditChainVerifyResult verify_audit_chain(domain_id, audit_chain_verify_request=audit_chain_verify_request)

Verify the per-Domain hash chain or a segment of it.

Recomputes `sha256(prev_hash || sha256(canonical_bytes))` for
every row in the requested segment and reports the first
divergence, if any. Omit both `from_seq`
and `to_seq` to verify the full chain anchored at the genesis
row (`seq=1`, `prev_hash` = 32 zero bytes); supply one or both
to verify a bounded segment without paging the entire chain
into memory.

DIVERGENCE IS NOT AN ERROR. A tampered chain surfaces as
`200 { ok: false, divergent_seq, expected_hash, observed_hash }`
— the handler MUST NEVER return a 5xx for tampering. The 5xx
channel is reserved for infrastructure faults (database
unreachable, decode error on a malformed row); divergence is
a finding the handler hands the operator alongside `200`. This
distinction matters because divergence triggers an incident
playbook while a 5xx triggers an oncall page.


### Example


```python
import plexsphere
from plexsphere.models.audit_chain_verify_request import AuditChainVerifyRequest
from plexsphere.models.audit_chain_verify_result import AuditChainVerifyResult
from plexsphere.rest import ApiException
from pprint import pprint

# Defining the host is optional and defaults to http://localhost
# See configuration.py for a list of all supported configuration parameters.
configuration = plexsphere.Configuration(
    host = "http://localhost"
)


# Enter a context with an instance of the API client
with plexsphere.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = plexsphere.AuditApi(api_client)
    domain_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | Owning Domain. The Platform Audit Log is per-Domain by residency contract — cross-Domain decisions land in EACH affected Domain's chain, never on a shared system chain . Every audit endpoint requires the Domain identifier in the path. 
    audit_chain_verify_request = {"from_seq":1,"to_seq":1000} # AuditChainVerifyRequest |  (optional)

    try:
        # Verify the per-Domain hash chain or a segment of it.
        api_response = api_instance.verify_audit_chain(domain_id, audit_chain_verify_request=audit_chain_verify_request)
        print("The response of AuditApi->verify_audit_chain:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling AuditApi->verify_audit_chain: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **domain_id** | **UUID**| Owning Domain. The Platform Audit Log is per-Domain by residency contract — cross-Domain decisions land in EACH affected Domain&#39;s chain, never on a shared system chain . Every audit endpoint requires the Domain identifier in the path.  | 
 **audit_chain_verify_request** | [**AuditChainVerifyRequest**](AuditChainVerifyRequest.md)|  | [optional] 

### Return type

[**AuditChainVerifyResult**](AuditChainVerifyResult.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Verifier completed. Inspect &#x60;ok&#x60; to decide whether the chain is healthy. On &#x60;ok: false&#x60;, &#x60;divergent_seq&#x60;, &#x60;expected_hash&#x60;, and &#x60;observed_hash&#x60; pin the offending row.  |  -  |
**400** | Invalid verify request (e.g. &#x60;to_seq&#x60; &lt; &#x60;from_seq&#x60;, negative seq).  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the &#x60;auditor&#x60; ReBAC relation on the addressed Domain.  |  -  |
**404** | Domain id not found. Surfaced only after the authorisation gate has passed.  |  -  |
**500** | Internal error: persistence-layer failure, authz-check crash, or canonical-encode crash on the verifier path. DIVERGENCE IS NOT A 5xx — see the operation description .  |  -  |
**501** | Audit chain store / authorizer not yet provisioned in this build. Body is a Problem with &#x60;code: audit_not_provisioned&#x60;.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **verify_platform_audit_chain**
> AuditChainVerifyResult verify_platform_audit_chain(audit_chain_verify_request=audit_chain_verify_request)

Verify the platform-residency hash chain or a segment of it.

Recomputes `sha256(prev_hash || sha256(canonical_bytes))` for
every row in the requested segment of the platform-residency
chain and reports the first divergence, if any. Omit both
`from_seq` and `to_seq` to verify the full chain anchored at
the genesis row (`seq=1`, `prev_hash` = 32 zero bytes);
supply one or both to verify a bounded segment without paging
the entire chain into memory.

DIVERGENCE IS NOT AN ERROR. A tampered chain surfaces as
`200 { ok: false, divergent_seq, expected_hash, observed_hash }`
— the handler MUST NEVER return a 5xx for tampering. The 5xx
channel is reserved for infrastructure faults (database
unreachable, decode error on a malformed row); divergence is
a finding the handler hands the operator alongside `200`. This
distinction matters because divergence triggers an incident
playbook while a 5xx triggers an oncall page.


### Example


```python
import plexsphere
from plexsphere.models.audit_chain_verify_request import AuditChainVerifyRequest
from plexsphere.models.audit_chain_verify_result import AuditChainVerifyResult
from plexsphere.rest import ApiException
from pprint import pprint

# Defining the host is optional and defaults to http://localhost
# See configuration.py for a list of all supported configuration parameters.
configuration = plexsphere.Configuration(
    host = "http://localhost"
)


# Enter a context with an instance of the API client
with plexsphere.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = plexsphere.AuditApi(api_client)
    audit_chain_verify_request = {from_seq=1, to_seq=1000} # AuditChainVerifyRequest |  (optional)

    try:
        # Verify the platform-residency hash chain or a segment of it.
        api_response = api_instance.verify_platform_audit_chain(audit_chain_verify_request=audit_chain_verify_request)
        print("The response of AuditApi->verify_platform_audit_chain:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling AuditApi->verify_platform_audit_chain: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **audit_chain_verify_request** | [**AuditChainVerifyRequest**](AuditChainVerifyRequest.md)|  | [optional] 

### Return type

[**AuditChainVerifyResult**](AuditChainVerifyResult.md)

### Authorization

No authorization required

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json, application/problem+json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Verifier completed. Inspect &#x60;ok&#x60; to decide whether the chain is healthy. On &#x60;ok: false&#x60;, &#x60;divergent_seq&#x60;, &#x60;expected_hash&#x60;, and &#x60;observed_hash&#x60; pin the offending row.  |  -  |
**400** | Invalid verify request (e.g. &#x60;to_seq&#x60; &lt; &#x60;from_seq&#x60;, negative seq).  |  -  |
**401** | Caller is not authenticated. |  -  |
**403** | Caller lacks the &#x60;auditor&#x60; ReBAC relation on the platform object.  |  -  |
**500** | Internal error: persistence-layer failure, authz-check crash, or canonical-encode crash on the verifier path. DIVERGENCE IS NOT A 5xx — see the operation description .  |  -  |
**501** | Audit chain store / authorizer not yet provisioned in this build. Body is a Problem with &#x60;code: audit_not_provisioned&#x60;.  |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

