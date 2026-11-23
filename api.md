# OmniWrap .NET API

<!-- omniwrap-api-reference eyJjb250cmFjdF9maW5nZXJwcmludCI6IjIyODliY2IwZDY2MWJkZWE2ZGQ0N2M3OTY3ZjM3YWFjZTdkM2ExNTJlMThlN2QyZjY0YzA4NTE0ZmI3M2U3OTUiLCJvcGVyYXRpb25fY291bnQiOjE2LCJyZW5kZXJlciI6ImRvdG5ldCIsInZlcnNpb24iOiIxLjUuMCJ9 -->

This generated index is tied to Semantic IR `1.14.0` and contract fingerprint `2289bcb0d661bdea6dd47c7967f37aace7d3a152e18e7d2f64c08514fb73e795`. Every operation below is rendered from the same semantic contract and artifact-attested public member allocation as the SDK.

SDK maturity: **experimental**  
Artifact evidence: [`omniwrap-artifact.json`](omniwrap-artifact.json) binds this file by SHA-256 and is revalidated by the package gate.

## Client hooks and tracing

`new ClientOptions { Hooks = new RequestHooks { ... } }` registers four callbacks. Every attempt starts with `BeforeRequest` and ends with exactly one of `OnRetry` (another attempt follows), `AfterResponse` (the success the call returns) or `OnError` (the failure the call reports). Each receives the operation id, the HTTP method, the URL, the request headers, the attempt number (from 1), the time elapsed since the call started and, once the server answered, the HTTP status. No context carries a credential: `Authorization`, `Proxy-Authorization`, `Cookie`, every API-key header and API-key query parameter, and any configured credential value read `[redacted]`. Every context is an immutable `HookContext` record: no hook can change the request.

`new ClientOptions { Tracer = ... }` opens one span per call through the tracer interface this SDK defines, named after the operation id, with `http.request.method`, `http.response.status_code` and `http.request.resend_count`. It needs no tracing library; README.md maps it onto the OpenTelemetry API.

## Shared types

- `ApiError`
- `Artifact`
- `CancelRequest`
- `CancellationReceipt`
- `ConfigurationRequest`
- `ConfigurationRevision`
- `Cursor`
- `Diagnostic`
- `ErrorDetail`
- `EventPage`
- `Evidence`
- `EvidenceBinding`
- `EvidencePage`
- `ExecutionBinding`
- `GateReport`
- `GateStatus`
- `LogEntry`
- `LogPage`
- `ObservedPhase`
- `OpaqueId`
- `Preview`
- `PreviewEvent`
- `PreviewInputs`
- `PreviewPage`
- `PreviewRequest`
- `PreviewState`
- `PrimaryArtifacts`
- `ProfileId`
- `Project`
- `ProjectPage`
- `Sha256`
- `SourceRevision`
- `StrongEtag`
- `Timestamp`
- `UserContext`

## Resource `Artifacts`

<!-- omniwrap-api-operation eyJleGFtcGxlX3JlYXNvbiI6bnVsbCwiZXhhbXBsZV9zdGF0dXMiOiJnZW5lcmF0ZWQiLCJtZW1iZXIiOiJBcnRpZmFjdHMuR2V0QXN5bmMiLCJtZXRob2QiOiJHRVQiLCJvcGVyYXRpb25faWQiOiJnZXRBcnRpZmFjdCIsInBhdGgiOiIvdjEvcHJvamVjdHMve3Byb2plY3RJZH0vYXJ0aWZhY3RzL3thcnRpZmFjdElkfSIsInJlc291cmNlIjoiQXJ0aWZhY3RzIiwic2VjdGlvbl9zaGEyNTYiOiJhZjZlZDljMzBlYzVkNTk4YWQ5M2JlNmY2OGVmNWIyNmY4NDUzMTkxNzE4ZGFiMzc2ZWFkN2RiMWUyMmU5ZjVjIiwic2lnbmF0dXJlX3NoYTI1NiI6IjQ2YTA5MzkxODI4MTUyZTkxM2U5MjU0YTcxNTlkOTJhY2MwZjJkMGVjZTE0ZmIzYWJkMmJjMGU4MzliYjZmYjkifQ -->
### `client.Artifacts.GetAsync`

Operation ID: `getArtifact`  
Route: `GET` `/v1/projects/{projectId}/artifacts/{artifactId}`  
Equivalent member: `client.GetArtifactAsync`  

#### Signature

```csharp
public Task<Artifact> GetAsync(
string projectId,
string artifactId,
RequestOptions? requestOptions = null,
CancellationToken cancellationToken = default)
```

#### Parameters

| Name | In | Required | Contract type | Description |
| --- | --- | --- | --- | --- |
| `projectId` | `path` | yes | `OpaqueId` | Opaque identity; authorize organization/project and verify every parent relation before lookup or mutation. Knowledge of an ID grants no access. |
| `artifactId` | `path` | yes | `OpaqueId` | Opaque identity; authorize organization/project and verify every parent relation before lookup or mutation. Knowledge of an ID grants no access. |

#### Responses

| Status | Description | Content |
| --- | --- | --- |
| `200` | Authorized resource snapshot. | application/json (Artifact) |
| `401` | unauthenticated; recovery=authenticate. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `403` | forbidden; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `404` | not_found; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `406` | not_acceptable; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `422` | invalid_request, source_not_valid or cursor_invalid; never reinterpret an invalid source as ready. | application/json (allOf&lt;ApiError, object&gt;) |
| `429` | rate_limited; recovery=retry_same_request. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `500` | internal_error; recovery=reconcile_idempotency. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `503` | configuration_required/configure_authority when approved JackAI exchange or product configuration is absent; authority_unavailable/retry_same_request on outage/timeout. No fallback identity, cached authorization or synthetic data. | application/json (allOf&lt;ApiError, object&gt;) |

#### Canonical required-input example

```json
{
  "parameters": [
    {
      "in": "path",
      "name": "projectId",
      "value": "example"
    },
    {
      "in": "path",
      "name": "artifactId",
      "value": "example"
    }
  ]
}
```

#### SDK call example

Example status: `generated`  
```csharp
var result = await client.Artifacts.GetAsync(projectId: "example", artifactId: "example");
```

<!-- omniwrap-api-operation-end eyJvcGVyYXRpb25faWQiOiJnZXRBcnRpZmFjdCJ9 -->

## Resource `Configurations`

<!-- omniwrap-api-operation eyJleGFtcGxlX3JlYXNvbiI6bnVsbCwiZXhhbXBsZV9zdGF0dXMiOiJnZW5lcmF0ZWQiLCJtZW1iZXIiOiJDb25maWd1cmF0aW9ucy5DcmVhdGVSZXZpc2lvbkFzeW5jIiwibWV0aG9kIjoiUE9TVCIsIm9wZXJhdGlvbl9pZCI6ImNyZWF0ZUNvbmZpZ3VyYXRpb25SZXZpc2lvbiIsInBhdGgiOiIvdjEvcHJvamVjdHMve3Byb2plY3RJZH0vY29uZmlndXJhdGlvbi1yZXZpc2lvbnMiLCJyZXNvdXJjZSI6IkNvbmZpZ3VyYXRpb25zIiwic2VjdGlvbl9zaGEyNTYiOiI3NjJiNDQ1M2UyYjM5YWZmZWIzZjhkY2M4NTJlMDVkNDg1YzZlOTkxYWQwZDNlMmZjZDBiNjZiZWQ4MGY2NGIzIiwic2lnbmF0dXJlX3NoYTI1NiI6IjJiZTY0YzIzYmQzMjY0MjUyZDA1NDlkNDg5YTI1ZTc2ZGEwMDNiNWU1ODgzMGZmMzk2MjFhMDY2MTg2MDYyMTAifQ -->
### `client.Configurations.CreateRevisionAsync`

Operation ID: `createConfigurationRevision`  
Route: `POST` `/v1/projects/{projectId}/configuration-revisions`  
Equivalent member: `client.CreateConfigurationRevisionAsync`  

#### Signature

```csharp
public Task<ConfigurationRevision> CreateRevisionAsync(
string projectId,
string ifMatch,
string idempotencyKey,
string xCSRFToken,
ConfigurationRequest body,
RequestOptions? requestOptions = null,
CancellationToken cancellationToken = default)
```

#### Parameters

| Name | In | Required | Contract type | Description |
| --- | --- | --- | --- | --- |
| `projectId` | `path` | yes | `OpaqueId` | Opaque identity; authorize organization/project and verify every parent relation before lookup or mutation. Knowledge of an ID grants no access. |
| `If-Match` | `header` | yes | `StrongEtag` | Exactly one strong ETag. Configuration activation and preview creation compare the current Project ETag, including first activation. Cancellation compares the Preview ETag. Stale = 412; absent = 428. Wildcard, weak tags and lists are rejected. |
| `Idempotency-Key` | `header` | yes | `string` | Required on every mutation. Scope = organization + project + operation; fingerprint includes raw body bytes, media type, resource and If-Match. Same key/content replays the same receipt; changed content is 409. Retained for 86400 seconds; reconcile an ambiguous outcome within retention before retry, never silently allocate a new key. |
| `X-CSRF-Token` | `header` | yes | `string` | Session-bound synchronizer token from the trusted BFF bootstrap, independent of the unreadable session cookie. Validate same-origin Origin and CSRF before mutation. This contract does not implement or provision the session/CSRF exchange. |
| `body` | body | yes | `ConfigurationRequest` |  |

#### Responses

| Status | Description | Content |
| --- | --- | --- |
| `201` | Created and atomically activated after admitted engine resolution; response ETag identifies the immutable configuration. Reload Project to obtain the new Project ETag. | application/json (ConfigurationRevision) |
| `401` | unauthenticated; recovery=authenticate. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `403` | forbidden; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `404` | not_found; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `406` | not_acceptable; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `409` | idempotency_conflict/reconcile_idempotency, binding_conflict/refresh_snapshot, or retry_requires_reconciliation/refresh_snapshot. No additional effect. | application/json (allOf&lt;ApiError, object&gt;) |
| `412` | precondition_failed; recovery=refresh_snapshot. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `413` | request_too_large; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `415` | unsupported_media_type; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `422` | invalid_request, source_not_valid or cursor_invalid; never reinterpret an invalid source as ready. | application/json (allOf&lt;ApiError, object&gt;) |
| `428` | precondition_required; recovery=refresh_snapshot. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `429` | rate_limited; recovery=retry_same_request. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `500` | internal_error; recovery=reconcile_idempotency. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `503` | configuration_required/configure_authority when approved JackAI exchange or product configuration is absent; authority_unavailable/retry_same_request on outage/timeout. No fallback identity, cached authorization or synthetic data. | application/json (allOf&lt;ApiError, object&gt;) |

#### Canonical required-input example

```json
{
  "body": {
    "profileId": "openapi-typescript-preview/1",
    "sourceRevisionId": "example"
  },
  "content_type": "application/json",
  "parameters": [
    {
      "in": "path",
      "name": "projectId",
      "value": "example"
    },
    {
      "in": "header",
      "name": "If-Match",
      "value": "\"A\""
    },
    {
      "in": "header",
      "name": "Idempotency-Key",
      "value": "AAAAAAAAAAAAAAAA"
    },
    {
      "in": "header",
      "name": "X-CSRF-Token",
      "value": "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA"
    }
  ]
}
```

#### SDK call example

Example status: `generated`  
```csharp
var result = await client.Configurations.CreateRevisionAsync(projectId: "example", If-Match: "\"A\"", Idempotency-Key: "AAAAAAAAAAAAAAAA", X-CSRF-Token: "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA", body: {"profileId": "openapi-typescript-preview/1", "sourceRevisionId": "example"});
```

<!-- omniwrap-api-operation-end eyJvcGVyYXRpb25faWQiOiJjcmVhdGVDb25maWd1cmF0aW9uUmV2aXNpb24ifQ -->

<!-- omniwrap-api-operation eyJleGFtcGxlX3JlYXNvbiI6bnVsbCwiZXhhbXBsZV9zdGF0dXMiOiJnZW5lcmF0ZWQiLCJtZW1iZXIiOiJDb25maWd1cmF0aW9ucy5HZXRSZXZpc2lvbkFzeW5jIiwibWV0aG9kIjoiR0VUIiwib3BlcmF0aW9uX2lkIjoiZ2V0Q29uZmlndXJhdGlvblJldmlzaW9uIiwicGF0aCI6Ii92MS9wcm9qZWN0cy97cHJvamVjdElkfS9jb25maWd1cmF0aW9uLXJldmlzaW9ucy97cmV2aXNpb25JZH0iLCJyZXNvdXJjZSI6IkNvbmZpZ3VyYXRpb25zIiwic2VjdGlvbl9zaGEyNTYiOiI1MWY3NzhlMzNjOThkNWY3Yjk1YTA0YmEyM2M5YWNiYzdjOTQwYjdmYWQ4MWM5YzNmYzUxYmUyNDRlNmYyODJjIiwic2lnbmF0dXJlX3NoYTI1NiI6IjIzMTY2YmNkODNjOTc1NDIzMGQ0ZDM4OTBiNjU0MWUwZjQwODRlNDk2YmI2ZGJkNTBhODcyOWU5N2YwMTFlZDQifQ -->
### `client.Configurations.GetRevisionAsync`

Operation ID: `getConfigurationRevision`  
Route: `GET` `/v1/projects/{projectId}/configuration-revisions/{revisionId}`  
Equivalent member: `client.GetConfigurationRevisionAsync`  

#### Signature

```csharp
public Task<ConfigurationRevision> GetRevisionAsync(
string projectId,
string revisionId,
RequestOptions? requestOptions = null,
CancellationToken cancellationToken = default)
```

#### Parameters

| Name | In | Required | Contract type | Description |
| --- | --- | --- | --- | --- |
| `projectId` | `path` | yes | `OpaqueId` | Opaque identity; authorize organization/project and verify every parent relation before lookup or mutation. Knowledge of an ID grants no access. |
| `revisionId` | `path` | yes | `OpaqueId` | Opaque identity; authorize organization/project and verify every parent relation before lookup or mutation. Knowledge of an ID grants no access. |

#### Responses

| Status | Description | Content |
| --- | --- | --- |
| `200` | Authorized resource snapshot. | application/json (ConfigurationRevision) |
| `401` | unauthenticated; recovery=authenticate. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `403` | forbidden; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `404` | not_found; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `406` | not_acceptable; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `422` | invalid_request, source_not_valid or cursor_invalid; never reinterpret an invalid source as ready. | application/json (allOf&lt;ApiError, object&gt;) |
| `429` | rate_limited; recovery=retry_same_request. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `500` | internal_error; recovery=reconcile_idempotency. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `503` | configuration_required/configure_authority when approved JackAI exchange or product configuration is absent; authority_unavailable/retry_same_request on outage/timeout. No fallback identity, cached authorization or synthetic data. | application/json (allOf&lt;ApiError, object&gt;) |

#### Canonical required-input example

```json
{
  "parameters": [
    {
      "in": "path",
      "name": "projectId",
      "value": "example"
    },
    {
      "in": "path",
      "name": "revisionId",
      "value": "example"
    }
  ]
}
```

#### SDK call example

Example status: `generated`  
```csharp
var result = await client.Configurations.GetRevisionAsync(projectId: "example", revisionId: "example");
```

<!-- omniwrap-api-operation-end eyJvcGVyYXRpb25faWQiOiJnZXRDb25maWd1cmF0aW9uUmV2aXNpb24ifQ -->

## Resource `Context`

<!-- omniwrap-api-operation eyJleGFtcGxlX3JlYXNvbiI6bnVsbCwiZXhhbXBsZV9zdGF0dXMiOiJnZW5lcmF0ZWQiLCJtZW1iZXIiOiJDb250ZXh0LkdldFVzZXJBc3luYyIsIm1ldGhvZCI6IkdFVCIsIm9wZXJhdGlvbl9pZCI6ImdldFVzZXJDb250ZXh0IiwicGF0aCI6Ii92MS9tZS9jb250ZXh0IiwicmVzb3VyY2UiOiJDb250ZXh0Iiwic2VjdGlvbl9zaGEyNTYiOiIzNWY5MGUzODM0NGI2ZWMyZGNiZTNhNDI3NzM1OTFhZDc4MjgxZmUwYjRjNjRiNDJmYWQ1NGMxZDU5ZjYxMmIyIiwic2lnbmF0dXJlX3NoYTI1NiI6IjFlMDc3NGM2MTkyZjM1YTQzN2FkNDgzYzVlNDkxMjZhNDBhYzkwMjZmOGUyNDU4MWFhNzhkMWIzODI4MTljN2UifQ -->
### `client.Context.GetUserAsync`

Operation ID: `getUserContext`  
Route: `GET` `/v1/me/context`  
Equivalent member: `client.GetUserContextAsync`  

#### Signature

```csharp
public Task<UserContext> GetUserAsync(
RequestOptions? requestOptions = null,
CancellationToken cancellationToken = default)
```

#### Parameters

| Name | In | Required | Contract type | Description |
| --- | --- | --- | --- | --- |
| _none_ | | | | |

#### Responses

| Status | Description | Content |
| --- | --- | --- |
| `200` | Authorized resource snapshot. | application/json (UserContext) |
| `401` | unauthenticated; recovery=authenticate. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `403` | forbidden; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `404` | not_found; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `406` | not_acceptable; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `422` | invalid_request, source_not_valid or cursor_invalid; never reinterpret an invalid source as ready. | application/json (allOf&lt;ApiError, object&gt;) |
| `429` | rate_limited; recovery=retry_same_request. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `500` | internal_error; recovery=reconcile_idempotency. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `503` | configuration_required/configure_authority when approved JackAI exchange or product configuration is absent; authority_unavailable/retry_same_request on outage/timeout. No fallback identity, cached authorization or synthetic data. | application/json (allOf&lt;ApiError, object&gt;) |

#### Canonical required-input example

```json
{
  "parameters": []
}
```

#### SDK call example

Example status: `generated`  
```csharp
var result = await client.Context.GetUserAsync();
```

<!-- omniwrap-api-operation-end eyJvcGVyYXRpb25faWQiOiJnZXRVc2VyQ29udGV4dCJ9 -->

## Resource `Evidence`

<!-- omniwrap-api-operation eyJleGFtcGxlX3JlYXNvbiI6bnVsbCwiZXhhbXBsZV9zdGF0dXMiOiJnZW5lcmF0ZWQiLCJtZW1iZXIiOiJFdmlkZW5jZS5HZXRBc3luYyIsIm1ldGhvZCI6IkdFVCIsIm9wZXJhdGlvbl9pZCI6ImdldEV2aWRlbmNlIiwicGF0aCI6Ii92MS9wcm9qZWN0cy97cHJvamVjdElkfS9ldmlkZW5jZS97ZXZpZGVuY2VJZH0iLCJyZXNvdXJjZSI6IkV2aWRlbmNlIiwic2VjdGlvbl9zaGEyNTYiOiI5ODQwZjJlZWM5NjM0ZTM1Y2I0YzZiYWY1ZWVmNDM0ZmMzYjUzOGY5ZDY4MzYxNWMzNTg1MDNhNDYzYWMzMDMwIiwic2lnbmF0dXJlX3NoYTI1NiI6IjA3ZTE0Mzg4ZjE2NDdkNDY0MDM0OTEzMzJhMWQ2NjRiOTQzODRlYTA2ODJmNTVhNzExNjkyZGU2Mjc2OTBkOGMifQ -->
### `client.Evidence.GetAsync`

Operation ID: `getEvidence`  
Route: `GET` `/v1/projects/{projectId}/evidence/{evidenceId}`  
Equivalent member: `client.GetEvidenceAsync`  

#### Signature

```csharp
public Task<Evidence> GetAsync(
string projectId,
string evidenceId,
RequestOptions? requestOptions = null,
CancellationToken cancellationToken = default)
```

#### Parameters

| Name | In | Required | Contract type | Description |
| --- | --- | --- | --- | --- |
| `projectId` | `path` | yes | `OpaqueId` | Opaque identity; authorize organization/project and verify every parent relation before lookup or mutation. Knowledge of an ID grants no access. |
| `evidenceId` | `path` | yes | `OpaqueId` | Opaque identity; authorize organization/project and verify every parent relation before lookup or mutation. Knowledge of an ID grants no access. |

#### Responses

| Status | Description | Content |
| --- | --- | --- |
| `200` | Authorized resource snapshot. | application/json (Evidence) |
| `401` | unauthenticated; recovery=authenticate. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `403` | forbidden; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `404` | not_found; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `406` | not_acceptable; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `422` | invalid_request, source_not_valid or cursor_invalid; never reinterpret an invalid source as ready. | application/json (allOf&lt;ApiError, object&gt;) |
| `429` | rate_limited; recovery=retry_same_request. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `500` | internal_error; recovery=reconcile_idempotency. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `503` | configuration_required/configure_authority when approved JackAI exchange or product configuration is absent; authority_unavailable/retry_same_request on outage/timeout. No fallback identity, cached authorization or synthetic data. | application/json (allOf&lt;ApiError, object&gt;) |

#### Canonical required-input example

```json
{
  "parameters": [
    {
      "in": "path",
      "name": "projectId",
      "value": "example"
    },
    {
      "in": "path",
      "name": "evidenceId",
      "value": "example"
    }
  ]
}
```

#### SDK call example

Example status: `generated`  
```csharp
var result = await client.Evidence.GetAsync(projectId: "example", evidenceId: "example");
```

<!-- omniwrap-api-operation-end eyJvcGVyYXRpb25faWQiOiJnZXRFdmlkZW5jZSJ9 -->

<!-- omniwrap-api-operation eyJleGFtcGxlX3JlYXNvbiI6bnVsbCwiZXhhbXBsZV9zdGF0dXMiOiJnZW5lcmF0ZWQiLCJtZW1iZXIiOiJFdmlkZW5jZS5MaXN0UHJldmlld0FzeW5jIiwibWV0aG9kIjoiR0VUIiwib3BlcmF0aW9uX2lkIjoibGlzdFByZXZpZXdFdmlkZW5jZSIsInBhdGgiOiIvdjEvcHJvamVjdHMve3Byb2plY3RJZH0vcHJldmlld3Mve3ByZXZpZXdJZH0vZXZpZGVuY2UiLCJyZXNvdXJjZSI6IkV2aWRlbmNlIiwic2VjdGlvbl9zaGEyNTYiOiI5ZDQ3N2RmZDlkZDEwZGRjYzQ1MWM3Y2ZiNDUxYzRiMWE3MzVkZGNiOWJiZjdkZDljMWVhMjQ0MmZmZjEyMjEzIiwic2lnbmF0dXJlX3NoYTI1NiI6ImNjMDZjNjhkNGQyMTU2NDFmMWJhZjQ4ZjBiMjU5ZTUyMmNiMTllZmNjNTE3YmI3ZmViZjU3M2RhNzZhZTZjNGYifQ -->
### `client.Evidence.ListPreviewAsync`

Operation ID: `listPreviewEvidence`  
Route: `GET` `/v1/projects/{projectId}/previews/{previewId}/evidence`  
Equivalent member: `client.ListPreviewEvidenceAsync`  

#### Signature

```csharp
public Task<EvidencePage> ListPreviewAsync(
string projectId,
string previewId,
string? cursor = null,
long? limit = null,
string? gateId = null,
ListPreviewEvidenceAvailability? availability = null,
RequestOptions? requestOptions = null,
CancellationToken cancellationToken = default)
```

#### Parameters

| Name | In | Required | Contract type | Description |
| --- | --- | --- | --- | --- |
| `projectId` | `path` | yes | `OpaqueId` | Opaque identity; authorize organization/project and verify every parent relation before lookup or mutation. Knowledge of an ID grants no access. |
| `previewId` | `path` | yes | `OpaqueId` | Opaque identity; authorize organization/project and verify every parent relation before lookup or mutation. Knowledge of an ID grants no access. |
| `cursor` | `query` | no | `Cursor` | Opaque scope-bound continuation, never an offset or authority token. Expired/gap = 410 refresh_snapshot; invalid/tampered/wrong filters = 422. Denied objects remain uniform404. |
| `limit` | `query` | no | `integer` | Maximum page size; default 50. |
| `gateId` | `query` | no | `string` |  |
| `availability` | `query` | no | `string` |  |

#### Responses

| Status | Description | Content |
| --- | --- | --- |
| `200` | Authorized resource snapshot. | application/json (EvidencePage) |
| `401` | unauthenticated; recovery=authenticate. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `403` | forbidden; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `404` | not_found; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `406` | not_acceptable; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `410` | cursor_expired; recovery=refresh_snapshot. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `422` | invalid_request, source_not_valid or cursor_invalid; never reinterpret an invalid source as ready. | application/json (allOf&lt;ApiError, object&gt;) |
| `429` | rate_limited; recovery=retry_same_request. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `500` | internal_error; recovery=reconcile_idempotency. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `503` | configuration_required/configure_authority when approved JackAI exchange or product configuration is absent; authority_unavailable/retry_same_request on outage/timeout. No fallback identity, cached authorization or synthetic data. | application/json (allOf&lt;ApiError, object&gt;) |

#### Canonical required-input example

```json
{
  "parameters": [
    {
      "in": "path",
      "name": "projectId",
      "value": "example"
    },
    {
      "in": "path",
      "name": "previewId",
      "value": "example"
    }
  ]
}
```

#### SDK call example

Example status: `generated`  
```csharp
var result = await client.Evidence.ListPreviewAsync(projectId: "example", previewId: "example", /* optional: cursor, limit, gateId, availability */);
```

<!-- omniwrap-api-operation-end eyJvcGVyYXRpb25faWQiOiJsaXN0UHJldmlld0V2aWRlbmNlIn0 -->

## Resource `Previews`

<!-- omniwrap-api-operation eyJleGFtcGxlX3JlYXNvbiI6bnVsbCwiZXhhbXBsZV9zdGF0dXMiOiJnZW5lcmF0ZWQiLCJtZW1iZXIiOiJQcmV2aWV3cy5DYW5jZWxBc3luYyIsIm1ldGhvZCI6IlBPU1QiLCJvcGVyYXRpb25faWQiOiJjYW5jZWxQcmV2aWV3IiwicGF0aCI6Ii92MS9wcm9qZWN0cy97cHJvamVjdElkfS9wcmV2aWV3cy97cHJldmlld0lkfS9jYW5jZWwiLCJyZXNvdXJjZSI6IlByZXZpZXdzIiwic2VjdGlvbl9zaGEyNTYiOiJiMGI4ZjBiMWJmOWVmMzA0MTgxMzk3OWUwYjQ2OTJjMWUxZmVkZjFlODE1YTcyODIyMzZiZGI0NmU2MGUzNTE5Iiwic2lnbmF0dXJlX3NoYTI1NiI6IjQ1ODMxMDQ0MWE1MGNiMGNiNDQ1OTc5MTExODBiMzdhM2FjYmY5YzRiMDk3ZGFkM2NkZDVhY2QyZTg3NTZjN2MifQ -->
### `client.Previews.CancelAsync`

Operation ID: `cancelPreview`  
Route: `POST` `/v1/projects/{projectId}/previews/{previewId}/cancel`  
Equivalent member: `client.CancelPreviewAsync`  

#### Signature

```csharp
public Task<CancellationReceipt> CancelAsync(
string projectId,
string previewId,
string ifMatch,
string idempotencyKey,
string xCSRFToken,
CancelRequest body,
RequestOptions? requestOptions = null,
CancellationToken cancellationToken = default)
```

#### Parameters

| Name | In | Required | Contract type | Description |
| --- | --- | --- | --- | --- |
| `projectId` | `path` | yes | `OpaqueId` | Opaque identity; authorize organization/project and verify every parent relation before lookup or mutation. Knowledge of an ID grants no access. |
| `previewId` | `path` | yes | `OpaqueId` | Opaque identity; authorize organization/project and verify every parent relation before lookup or mutation. Knowledge of an ID grants no access. |
| `If-Match` | `header` | yes | `StrongEtag` | Exactly one strong ETag. Configuration activation and preview creation compare the current Project ETag, including first activation. Cancellation compares the Preview ETag. Stale = 412; absent = 428. Wildcard, weak tags and lists are rejected. |
| `Idempotency-Key` | `header` | yes | `string` | Required on every mutation. Scope = organization + project + operation; fingerprint includes raw body bytes, media type, resource and If-Match. Same key/content replays the same receipt; changed content is 409. Retained for 86400 seconds; reconcile an ambiguous outcome within retention before retry, never silently allocate a new key. |
| `X-CSRF-Token` | `header` | yes | `string` | Session-bound synchronizer token from the trusted BFF bootstrap, independent of the unreadable session cookie. Validate same-origin Origin and CSRF before mutation. This contract does not implement or provision the session/CSRF exchange. |
| `body` | body | yes | `CancelRequest` |  |

#### Responses

| Status | Description | Content |
| --- | --- | --- |
| `202` | Accepted for asynchronous processing; not a completed effect. | application/json (CancellationReceipt) |
| `401` | unauthenticated; recovery=authenticate. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `403` | forbidden; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `404` | not_found; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `406` | not_acceptable; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `409` | idempotency_conflict/reconcile_idempotency, binding_conflict/refresh_snapshot, or retry_requires_reconciliation/refresh_snapshot. No additional effect. | application/json (allOf&lt;ApiError, object&gt;) |
| `412` | precondition_failed; recovery=refresh_snapshot. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `413` | request_too_large; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `415` | unsupported_media_type; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `422` | invalid_request, source_not_valid or cursor_invalid; never reinterpret an invalid source as ready. | application/json (allOf&lt;ApiError, object&gt;) |
| `428` | precondition_required; recovery=refresh_snapshot. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `429` | rate_limited; recovery=retry_same_request. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `500` | internal_error; recovery=reconcile_idempotency. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `503` | configuration_required/configure_authority when approved JackAI exchange or product configuration is absent; authority_unavailable/retry_same_request on outage/timeout. No fallback identity, cached authorization or synthetic data. | application/json (allOf&lt;ApiError, object&gt;) |

#### Canonical required-input example

```json
{
  "body": {
    "reason": "user_requested"
  },
  "content_type": "application/json",
  "parameters": [
    {
      "in": "path",
      "name": "projectId",
      "value": "example"
    },
    {
      "in": "path",
      "name": "previewId",
      "value": "example"
    },
    {
      "in": "header",
      "name": "If-Match",
      "value": "\"A\""
    },
    {
      "in": "header",
      "name": "Idempotency-Key",
      "value": "AAAAAAAAAAAAAAAA"
    },
    {
      "in": "header",
      "name": "X-CSRF-Token",
      "value": "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA"
    }
  ]
}
```

#### SDK call example

Example status: `generated`  
```csharp
var result = await client.Previews.CancelAsync(projectId: "example", previewId: "example", If-Match: "\"A\"", Idempotency-Key: "AAAAAAAAAAAAAAAA", X-CSRF-Token: "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA", body: {"reason": "user_requested"});
```

<!-- omniwrap-api-operation-end eyJvcGVyYXRpb25faWQiOiJjYW5jZWxQcmV2aWV3In0 -->

<!-- omniwrap-api-operation eyJleGFtcGxlX3JlYXNvbiI6bnVsbCwiZXhhbXBsZV9zdGF0dXMiOiJnZW5lcmF0ZWQiLCJtZW1iZXIiOiJQcmV2aWV3cy5DcmVhdGVBc3luYyIsIm1ldGhvZCI6IlBPU1QiLCJvcGVyYXRpb25faWQiOiJjcmVhdGVQcmV2aWV3IiwicGF0aCI6Ii92MS9wcm9qZWN0cy97cHJvamVjdElkfS9wcmV2aWV3cyIsInJlc291cmNlIjoiUHJldmlld3MiLCJzZWN0aW9uX3NoYTI1NiI6IjdlNjc1MGQ4YWZiMWQxMjdiYjIxZGQ5ZDQ4MGEzYzcwZWE3OTZhNzZhYjhmYjFmZTVhYjc5ZmM5OWYzNDAyZWIiLCJzaWduYXR1cmVfc2hhMjU2IjoiMjhlMmI4ZTYzYTg5OWZkMWFiN2IzZGVhODMwNjU1NzFlMWZiNTMzYjIwYjA0NDU5MTkwMjIwOTAwZTdlNjY3NSJ9 -->
### `client.Previews.CreateAsync`

Operation ID: `createPreview`  
Route: `POST` `/v1/projects/{projectId}/previews`  
Equivalent member: `client.CreatePreviewAsync`  

#### Signature

```csharp
public Task<Preview> CreateAsync(
string projectId,
string ifMatch,
string idempotencyKey,
string xCSRFToken,
PreviewRequest body,
RequestOptions? requestOptions = null,
CancellationToken cancellationToken = default)
```

#### Parameters

| Name | In | Required | Contract type | Description |
| --- | --- | --- | --- | --- |
| `projectId` | `path` | yes | `OpaqueId` | Opaque identity; authorize organization/project and verify every parent relation before lookup or mutation. Knowledge of an ID grants no access. |
| `If-Match` | `header` | yes | `StrongEtag` | Exactly one strong ETag. Configuration activation and preview creation compare the current Project ETag, including first activation. Cancellation compares the Preview ETag. Stale = 412; absent = 428. Wildcard, weak tags and lists are rejected. |
| `Idempotency-Key` | `header` | yes | `string` | Required on every mutation. Scope = organization + project + operation; fingerprint includes raw body bytes, media type, resource and If-Match. Same key/content replays the same receipt; changed content is 409. Retained for 86400 seconds; reconcile an ambiguous outcome within retention before retry, never silently allocate a new key. |
| `X-CSRF-Token` | `header` | yes | `string` | Session-bound synchronizer token from the trusted BFF bootstrap, independent of the unreadable session cookie. Validate same-origin Origin and CSRF before mutation. This contract does not implement or provision the session/CSRF exchange. |
| `body` | body | yes | `PreviewRequest` |  |

#### Responses

| Status | Description | Content |
| --- | --- | --- |
| `202` | Accepted for asynchronous processing; not a completed effect. | application/json (Preview) |
| `401` | unauthenticated; recovery=authenticate. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `403` | forbidden; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `404` | not_found; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `406` | not_acceptable; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `409` | idempotency_conflict/reconcile_idempotency, binding_conflict/refresh_snapshot, or retry_requires_reconciliation/refresh_snapshot. No additional effect. | application/json (allOf&lt;ApiError, object&gt;) |
| `412` | precondition_failed; recovery=refresh_snapshot. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `413` | request_too_large; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `415` | unsupported_media_type; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `422` | invalid_request, source_not_valid or cursor_invalid; never reinterpret an invalid source as ready. | application/json (allOf&lt;ApiError, object&gt;) |
| `428` | precondition_required; recovery=refresh_snapshot. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `429` | rate_limited; recovery=retry_same_request. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `500` | internal_error; recovery=reconcile_idempotency. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `503` | configuration_required/configure_authority when approved JackAI exchange or product configuration is absent; authority_unavailable/retry_same_request on outage/timeout. No fallback identity, cached authorization or synthetic data. | application/json (allOf&lt;ApiError, object&gt;) |

#### Canonical required-input example

```json
{
  "body": {
    "configurationRevisionId": "example"
  },
  "content_type": "application/json",
  "parameters": [
    {
      "in": "path",
      "name": "projectId",
      "value": "example"
    },
    {
      "in": "header",
      "name": "If-Match",
      "value": "\"A\""
    },
    {
      "in": "header",
      "name": "Idempotency-Key",
      "value": "AAAAAAAAAAAAAAAA"
    },
    {
      "in": "header",
      "name": "X-CSRF-Token",
      "value": "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA"
    }
  ]
}
```

#### SDK call example

Example status: `generated`  
```csharp
var result = await client.Previews.CreateAsync(projectId: "example", If-Match: "\"A\"", Idempotency-Key: "AAAAAAAAAAAAAAAA", X-CSRF-Token: "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA", body: {"configurationRevisionId": "example"});
```

<!-- omniwrap-api-operation-end eyJvcGVyYXRpb25faWQiOiJjcmVhdGVQcmV2aWV3In0 -->

<!-- omniwrap-api-operation eyJleGFtcGxlX3JlYXNvbiI6bnVsbCwiZXhhbXBsZV9zdGF0dXMiOiJnZW5lcmF0ZWQiLCJtZW1iZXIiOiJQcmV2aWV3cy5HZXRBc3luYyIsIm1ldGhvZCI6IkdFVCIsIm9wZXJhdGlvbl9pZCI6ImdldFByZXZpZXciLCJwYXRoIjoiL3YxL3Byb2plY3RzL3twcm9qZWN0SWR9L3ByZXZpZXdzL3twcmV2aWV3SWR9IiwicmVzb3VyY2UiOiJQcmV2aWV3cyIsInNlY3Rpb25fc2hhMjU2IjoiMWRlYzMzZDQwMWZkZGVkMzYxYzg5ZjQzNjBjYzIwNDMyNzJhNGZmOWY3ZmJkYjZlOTQ2MzZhYWVlNTgyOTg0MCIsInNpZ25hdHVyZV9zaGEyNTYiOiJmNjg2NjE2MGFjNTk0NDkzZjVjZTdkNTk1OTRlNDMyMzc5N2UxZTQ4NWM4ZmQ4MDZiN2IyYzI1OWVkYWI2YmYwIn0 -->
### `client.Previews.GetAsync`

Operation ID: `getPreview`  
Route: `GET` `/v1/projects/{projectId}/previews/{previewId}`  
Equivalent member: `client.GetPreviewAsync`  

#### Signature

```csharp
public Task<Preview> GetAsync(
string projectId,
string previewId,
RequestOptions? requestOptions = null,
CancellationToken cancellationToken = default)
```

#### Parameters

| Name | In | Required | Contract type | Description |
| --- | --- | --- | --- | --- |
| `projectId` | `path` | yes | `OpaqueId` | Opaque identity; authorize organization/project and verify every parent relation before lookup or mutation. Knowledge of an ID grants no access. |
| `previewId` | `path` | yes | `OpaqueId` | Opaque identity; authorize organization/project and verify every parent relation before lookup or mutation. Knowledge of an ID grants no access. |

#### Responses

| Status | Description | Content |
| --- | --- | --- |
| `200` | Authorized resource snapshot. | application/json (Preview) |
| `401` | unauthenticated; recovery=authenticate. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `403` | forbidden; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `404` | not_found; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `406` | not_acceptable; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `422` | invalid_request, source_not_valid or cursor_invalid; never reinterpret an invalid source as ready. | application/json (allOf&lt;ApiError, object&gt;) |
| `429` | rate_limited; recovery=retry_same_request. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `500` | internal_error; recovery=reconcile_idempotency. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `503` | configuration_required/configure_authority when approved JackAI exchange or product configuration is absent; authority_unavailable/retry_same_request on outage/timeout. No fallback identity, cached authorization or synthetic data. | application/json (allOf&lt;ApiError, object&gt;) |

#### Canonical required-input example

```json
{
  "parameters": [
    {
      "in": "path",
      "name": "projectId",
      "value": "example"
    },
    {
      "in": "path",
      "name": "previewId",
      "value": "example"
    }
  ]
}
```

#### SDK call example

Example status: `generated`  
```csharp
var result = await client.Previews.GetAsync(projectId: "example", previewId: "example");
```

<!-- omniwrap-api-operation-end eyJvcGVyYXRpb25faWQiOiJnZXRQcmV2aWV3In0 -->

<!-- omniwrap-api-operation eyJleGFtcGxlX3JlYXNvbiI6bnVsbCwiZXhhbXBsZV9zdGF0dXMiOiJnZW5lcmF0ZWQiLCJtZW1iZXIiOiJQcmV2aWV3cy5HZXRFdmVudHNBc3luYyIsIm1ldGhvZCI6IkdFVCIsIm9wZXJhdGlvbl9pZCI6ImdldFByZXZpZXdFdmVudHMiLCJwYXRoIjoiL3YxL3Byb2plY3RzL3twcm9qZWN0SWR9L3ByZXZpZXdzL3twcmV2aWV3SWR9L2V2ZW50cyIsInJlc291cmNlIjoiUHJldmlld3MiLCJzZWN0aW9uX3NoYTI1NiI6IjJjYzNlMWZlMWRlMzAwNWVlMjhlY2ViODIwZjc2Y2E2MzMwNDM3YzNmYTQzYmE3MDU4YWE4NTUxMTkzNjY2ZjYiLCJzaWduYXR1cmVfc2hhMjU2IjoiNzViMTdiNjYwOTIwNDgxOWVkZjg3MDk2YTdlMjkzNDczMmEzMWJkYWYyM2VjOTEyYWU2MzI4NTdiNzAwYWQyYiJ9 -->
### `client.Previews.GetEventsAsync`

Operation ID: `getPreviewEvents`  
Route: `GET` `/v1/projects/{projectId}/previews/{previewId}/events`  
Equivalent member: `client.GetPreviewEventsAsync`  

#### Signature

```csharp
public Task<EventPage> GetEventsAsync(
string projectId,
string previewId,
string? cursor = null,
long? limit = null,
string? lastEventID = null,
RequestOptions? requestOptions = null,
CancellationToken cancellationToken = default)
```

#### Parameters

| Name | In | Required | Contract type | Description |
| --- | --- | --- | --- | --- |
| `projectId` | `path` | yes | `OpaqueId` | Opaque identity; authorize organization/project and verify every parent relation before lookup or mutation. Knowledge of an ID grants no access. |
| `previewId` | `path` | yes | `OpaqueId` | Opaque identity; authorize organization/project and verify every parent relation before lookup or mutation. Knowledge of an ID grants no access. |
| `cursor` | `query` | no | `Cursor` | Opaque scope-bound continuation, never an offset or authority token. Expired/gap = 410 refresh_snapshot; invalid/tampered/wrong filters = 422. Denied objects remain uniform404. |
| `limit` | `query` | no | `integer` | Maximum page size; default 50. |
| `Last-Event-ID` | `header` | no | `Cursor` | SSE resume cursor; replay begins strictly after it. If query cursor also occurs the two must match, otherwise 422. Reauthorization is mandatory on reconnect. |

#### Responses

| Status | Description | Content |
| --- | --- | --- |
| `200` | Authorized resource snapshot. | application/json (EventPage), text/event-stream (string) |
| `401` | unauthenticated; recovery=authenticate. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `403` | forbidden; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `404` | not_found; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `406` | not_acceptable; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `410` | cursor_expired; recovery=refresh_snapshot. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `422` | invalid_request, source_not_valid or cursor_invalid; never reinterpret an invalid source as ready. | application/json (allOf&lt;ApiError, object&gt;) |
| `429` | rate_limited; recovery=retry_same_request. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `500` | internal_error; recovery=reconcile_idempotency. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `503` | configuration_required/configure_authority when approved JackAI exchange or product configuration is absent; authority_unavailable/retry_same_request on outage/timeout. No fallback identity, cached authorization or synthetic data. | application/json (allOf&lt;ApiError, object&gt;) |

#### Canonical required-input example

```json
{
  "parameters": [
    {
      "in": "path",
      "name": "projectId",
      "value": "example"
    },
    {
      "in": "path",
      "name": "previewId",
      "value": "example"
    }
  ]
}
```

#### SDK call example

Example status: `generated`  
```csharp
var result = await client.Previews.GetEventsAsync(projectId: "example", previewId: "example", /* optional: cursor, limit, Last-Event-ID */);
```

<!-- omniwrap-api-operation-end eyJvcGVyYXRpb25faWQiOiJnZXRQcmV2aWV3RXZlbnRzIn0 -->

<!-- omniwrap-api-operation eyJleGFtcGxlX3JlYXNvbiI6bnVsbCwiZXhhbXBsZV9zdGF0dXMiOiJnZW5lcmF0ZWQiLCJtZW1iZXIiOiJQcmV2aWV3cy5HZXRMb2dzQXN5bmMiLCJtZXRob2QiOiJHRVQiLCJvcGVyYXRpb25faWQiOiJnZXRQcmV2aWV3TG9ncyIsInBhdGgiOiIvdjEvcHJvamVjdHMve3Byb2plY3RJZH0vcHJldmlld3Mve3ByZXZpZXdJZH0vbG9ncyIsInJlc291cmNlIjoiUHJldmlld3MiLCJzZWN0aW9uX3NoYTI1NiI6IjY4YTZkYTRiZjE1NDAyM2Y3YmQwYzQyNTU2ZDk4NzdhYWYzZDg4NzFmYmUxY2U4NmQ0ZWNkZDdlMTI3YzU4MDkiLCJzaWduYXR1cmVfc2hhMjU2IjoiZGEwYmJiYmI0YTVhOTI3NTBjYTI1YjljYmQ0YmQ3Y2Y0NjcwOTdmZmNjOTYyNDA2ZDRiMzA2NjI5M2ZjZTA2MyJ9 -->
### `client.Previews.GetLogsAsync`

Operation ID: `getPreviewLogs`  
Route: `GET` `/v1/projects/{projectId}/previews/{previewId}/logs`  
Equivalent member: `client.GetPreviewLogsAsync`  

#### Signature

```csharp
public Task<LogPage> GetLogsAsync(
string projectId,
string previewId,
string? cursor = null,
long? limit = null,
GetPreviewLogsLevel? level = null,
string? phaseId = null,
RequestOptions? requestOptions = null,
CancellationToken cancellationToken = default)
```

#### Parameters

| Name | In | Required | Contract type | Description |
| --- | --- | --- | --- | --- |
| `projectId` | `path` | yes | `OpaqueId` | Opaque identity; authorize organization/project and verify every parent relation before lookup or mutation. Knowledge of an ID grants no access. |
| `previewId` | `path` | yes | `OpaqueId` | Opaque identity; authorize organization/project and verify every parent relation before lookup or mutation. Knowledge of an ID grants no access. |
| `cursor` | `query` | no | `Cursor` | Opaque scope-bound continuation, never an offset or authority token. Expired/gap = 410 refresh_snapshot; invalid/tampered/wrong filters = 422. Denied objects remain uniform404. |
| `limit` | `query` | no | `integer` | Maximum page size; default 50. |
| `level` | `query` | no | `string` |  |
| `phaseId` | `query` | no | `OpaqueId` |  |

#### Responses

| Status | Description | Content |
| --- | --- | --- |
| `200` | Authorized resource snapshot. | application/json (LogPage) |
| `401` | unauthenticated; recovery=authenticate. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `403` | forbidden; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `404` | not_found; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `406` | not_acceptable; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `410` | cursor_expired; recovery=refresh_snapshot. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `422` | invalid_request, source_not_valid or cursor_invalid; never reinterpret an invalid source as ready. | application/json (allOf&lt;ApiError, object&gt;) |
| `429` | rate_limited; recovery=retry_same_request. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `500` | internal_error; recovery=reconcile_idempotency. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `503` | configuration_required/configure_authority when approved JackAI exchange or product configuration is absent; authority_unavailable/retry_same_request on outage/timeout. No fallback identity, cached authorization or synthetic data. | application/json (allOf&lt;ApiError, object&gt;) |

#### Canonical required-input example

```json
{
  "parameters": [
    {
      "in": "path",
      "name": "projectId",
      "value": "example"
    },
    {
      "in": "path",
      "name": "previewId",
      "value": "example"
    }
  ]
}
```

#### SDK call example

Example status: `generated`  
```csharp
var result = await client.Previews.GetLogsAsync(projectId: "example", previewId: "example", /* optional: cursor, limit, level, phaseId */);
```

<!-- omniwrap-api-operation-end eyJvcGVyYXRpb25faWQiOiJnZXRQcmV2aWV3TG9ncyJ9 -->

<!-- omniwrap-api-operation eyJleGFtcGxlX3JlYXNvbiI6bnVsbCwiZXhhbXBsZV9zdGF0dXMiOiJnZW5lcmF0ZWQiLCJtZW1iZXIiOiJQcmV2aWV3cy5MaXN0QXN5bmMiLCJtZXRob2QiOiJHRVQiLCJvcGVyYXRpb25faWQiOiJsaXN0UHJldmlld3MiLCJwYXRoIjoiL3YxL3Byb2plY3RzL3twcm9qZWN0SWR9L3ByZXZpZXdzIiwicmVzb3VyY2UiOiJQcmV2aWV3cyIsInNlY3Rpb25fc2hhMjU2IjoiMWVkZmMzZmY1MGQ4OWNjZjY2OTBlNDU3NTMwMTM0YjMyODRlMmQwZWQzNzZhMGYxNjk2NWZlYWQ5MWU0MDNmNSIsInNpZ25hdHVyZV9zaGEyNTYiOiIwMjAzMzczNmUzNWUzNzdiYjkxY2IwMmRlNjcyNTY0MDJjZDc1Mzc5NzI0OGVmZThjZTM4N2IzYTg4ZDNkODBiIn0 -->
### `client.Previews.ListAsync`

Operation ID: `listPreviews`  
Route: `GET` `/v1/projects/{projectId}/previews`  
Equivalent member: `client.ListPreviewsAsync`  

#### Signature

```csharp
public Task<PreviewPage> ListAsync(
string projectId,
string? cursor = null,
long? limit = null,
RequestOptions? requestOptions = null,
CancellationToken cancellationToken = default)
```

#### Parameters

| Name | In | Required | Contract type | Description |
| --- | --- | --- | --- | --- |
| `projectId` | `path` | yes | `OpaqueId` | Opaque identity; authorize organization/project and verify every parent relation before lookup or mutation. Knowledge of an ID grants no access. |
| `cursor` | `query` | no | `Cursor` | Opaque scope-bound continuation, never an offset or authority token. Expired/gap = 410 refresh_snapshot; invalid/tampered/wrong filters = 422. Denied objects remain uniform404. |
| `limit` | `query` | no | `integer` | Maximum page size; default 50. |

#### Responses

| Status | Description | Content |
| --- | --- | --- |
| `200` | Authorized resource snapshot. | application/json (PreviewPage) |
| `401` | unauthenticated; recovery=authenticate. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `403` | forbidden; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `404` | not_found; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `406` | not_acceptable; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `410` | cursor_expired; recovery=refresh_snapshot. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `422` | invalid_request, source_not_valid or cursor_invalid; never reinterpret an invalid source as ready. | application/json (allOf&lt;ApiError, object&gt;) |
| `429` | rate_limited; recovery=retry_same_request. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `500` | internal_error; recovery=reconcile_idempotency. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `503` | configuration_required/configure_authority when approved JackAI exchange or product configuration is absent; authority_unavailable/retry_same_request on outage/timeout. No fallback identity, cached authorization or synthetic data. | application/json (allOf&lt;ApiError, object&gt;) |

#### Canonical required-input example

```json
{
  "parameters": [
    {
      "in": "path",
      "name": "projectId",
      "value": "example"
    }
  ]
}
```

#### SDK call example

Example status: `generated`  
```csharp
var result = await client.Previews.ListAsync(projectId: "example", /* optional: cursor, limit */);
```

<!-- omniwrap-api-operation-end eyJvcGVyYXRpb25faWQiOiJsaXN0UHJldmlld3MifQ -->

## Resource `Projects`

<!-- omniwrap-api-operation eyJleGFtcGxlX3JlYXNvbiI6bnVsbCwiZXhhbXBsZV9zdGF0dXMiOiJnZW5lcmF0ZWQiLCJtZW1iZXIiOiJQcm9qZWN0cy5HZXRBc3luYyIsIm1ldGhvZCI6IkdFVCIsIm9wZXJhdGlvbl9pZCI6ImdldFByb2plY3QiLCJwYXRoIjoiL3YxL3Byb2plY3RzL3twcm9qZWN0SWR9IiwicmVzb3VyY2UiOiJQcm9qZWN0cyIsInNlY3Rpb25fc2hhMjU2IjoiYTU5N2U4ODUzYTQ2NGRjMzM5ZGMzNzZjODNlY2U3Y2JiYTgwYTI3ODJjODA5OTVjOGQzNDNhZDE2NjM3OTQxNSIsInNpZ25hdHVyZV9zaGEyNTYiOiJjOGY4YWU5YWFkMTM3N2ZjMjg2MTNkOWFhNzZmZWY0YmI0MDcxY2U2ZmJkMGVkZWQ2ZjRkY2M4MDZhNzliMzc1In0 -->
### `client.Projects.GetAsync`

Operation ID: `getProject`  
Route: `GET` `/v1/projects/{projectId}`  
Equivalent member: `client.GetProjectAsync`  

#### Signature

```csharp
public Task<Project> GetAsync(
string projectId,
RequestOptions? requestOptions = null,
CancellationToken cancellationToken = default)
```

#### Parameters

| Name | In | Required | Contract type | Description |
| --- | --- | --- | --- | --- |
| `projectId` | `path` | yes | `OpaqueId` | Opaque identity; authorize organization/project and verify every parent relation before lookup or mutation. Knowledge of an ID grants no access. |

#### Responses

| Status | Description | Content |
| --- | --- | --- |
| `200` | Authorized resource snapshot. | application/json (Project) |
| `401` | unauthenticated; recovery=authenticate. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `403` | forbidden; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `404` | not_found; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `406` | not_acceptable; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `422` | invalid_request, source_not_valid or cursor_invalid; never reinterpret an invalid source as ready. | application/json (allOf&lt;ApiError, object&gt;) |
| `429` | rate_limited; recovery=retry_same_request. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `500` | internal_error; recovery=reconcile_idempotency. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `503` | configuration_required/configure_authority when approved JackAI exchange or product configuration is absent; authority_unavailable/retry_same_request on outage/timeout. No fallback identity, cached authorization or synthetic data. | application/json (allOf&lt;ApiError, object&gt;) |

#### Canonical required-input example

```json
{
  "parameters": [
    {
      "in": "path",
      "name": "projectId",
      "value": "example"
    }
  ]
}
```

#### SDK call example

Example status: `generated`  
```csharp
var result = await client.Projects.GetAsync(projectId: "example");
```

<!-- omniwrap-api-operation-end eyJvcGVyYXRpb25faWQiOiJnZXRQcm9qZWN0In0 -->

<!-- omniwrap-api-operation eyJleGFtcGxlX3JlYXNvbiI6bnVsbCwiZXhhbXBsZV9zdGF0dXMiOiJnZW5lcmF0ZWQiLCJtZW1iZXIiOiJQcm9qZWN0cy5MaXN0QXN5bmMiLCJtZXRob2QiOiJHRVQiLCJvcGVyYXRpb25faWQiOiJsaXN0UHJvamVjdHMiLCJwYXRoIjoiL3YxL3Byb2plY3RzIiwicmVzb3VyY2UiOiJQcm9qZWN0cyIsInNlY3Rpb25fc2hhMjU2IjoiZTUwODU1ZTg1YjI5YTg1NWVhZDFlMzEyMmVjNTViMzkzZWY5ZjIwNmNjMDE2MTg3MzJiNjYwMTU1ZWFkNDhmNyIsInNpZ25hdHVyZV9zaGEyNTYiOiI2MDZkYzRiYTAyNmE3MGVkZmMxOGEzOWI5MjE4ZDNmYWJlOTAwYWZhM2Y3YzhmNTM1MjA5OTc2ZGM2MWY3MTZlIn0 -->
### `client.Projects.ListAsync`

Operation ID: `listProjects`  
Route: `GET` `/v1/projects`  
Equivalent member: `client.ListProjectsAsync`  

#### Signature

```csharp
public Task<ProjectPage> ListAsync(
string? cursor = null,
long? limit = null,
string? queryX738b12b7b458 = null,
RequestOptions? requestOptions = null,
CancellationToken cancellationToken = default)
```

#### Parameters

| Name | In | Required | Contract type | Description |
| --- | --- | --- | --- | --- |
| `cursor` | `query` | no | `Cursor` | Opaque scope-bound continuation, never an offset or authority token. Expired/gap = 410 refresh_snapshot; invalid/tampered/wrong filters = 422. Denied objects remain uniform404. |
| `limit` | `query` | no | `integer` | Maximum page size; default 50. |
| `query` | `query` | no | `string` | Display-name substring filter, case-insensitive Unicode NFC matching. Bound to pagination cursor. |

#### Responses

| Status | Description | Content |
| --- | --- | --- |
| `200` | Authorized resource snapshot. | application/json (ProjectPage) |
| `401` | unauthenticated; recovery=authenticate. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `403` | forbidden; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `404` | not_found; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `406` | not_acceptable; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `410` | cursor_expired; recovery=refresh_snapshot. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `422` | invalid_request, source_not_valid or cursor_invalid; never reinterpret an invalid source as ready. | application/json (allOf&lt;ApiError, object&gt;) |
| `429` | rate_limited; recovery=retry_same_request. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `500` | internal_error; recovery=reconcile_idempotency. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `503` | configuration_required/configure_authority when approved JackAI exchange or product configuration is absent; authority_unavailable/retry_same_request on outage/timeout. No fallback identity, cached authorization or synthetic data. | application/json (allOf&lt;ApiError, object&gt;) |

#### Canonical required-input example

```json
{
  "parameters": []
}
```

#### SDK call example

Example status: `generated`  
```csharp
var result = await client.Projects.ListAsync(/* optional: cursor, limit, query */);
```

<!-- omniwrap-api-operation-end eyJvcGVyYXRpb25faWQiOiJsaXN0UHJvamVjdHMifQ -->

## Resource `Sources`

<!-- omniwrap-api-operation eyJleGFtcGxlX3JlYXNvbiI6bnVsbCwiZXhhbXBsZV9zdGF0dXMiOiJnZW5lcmF0ZWQiLCJtZW1iZXIiOiJTb3VyY2VzLkNyZWF0ZVJldmlzaW9uQXN5bmMiLCJtZXRob2QiOiJQT1NUIiwib3BlcmF0aW9uX2lkIjoiY3JlYXRlU291cmNlUmV2aXNpb24iLCJwYXRoIjoiL3YxL3Byb2plY3RzL3twcm9qZWN0SWR9L3NvdXJjZS1yZXZpc2lvbnMiLCJyZXNvdXJjZSI6IlNvdXJjZXMiLCJzZWN0aW9uX3NoYTI1NiI6IjI1YmYxZWQ4ODZkYmY2NmE0ZGQ5ZGNlOTg4ZGEzMDdlMzcyODhiMTM5NTIwOThmMzJiYTNjODAxYjMxZGIzMWEiLCJzaWduYXR1cmVfc2hhMjU2IjoiM2QxZWM4MzBhMWU0Y2Y2NjU5YTQ5YWNkY2JkMWI1ZDFkMWQ0N2ViNmE0Yzg2YWY4ZmViZGQwYmUyYTRlZTYxMSJ9 -->
### `client.Sources.CreateRevisionAsync`

Operation ID: `createSourceRevision`  
Route: `POST` `/v1/projects/{projectId}/source-revisions`  
Equivalent member: `client.CreateSourceRevisionAsync`  

#### Signature

```csharp
public Task<SourceRevision> CreateRevisionAsync(
string projectId,
string idempotencyKey,
string xCSRFToken,
string displayName,
JsonElement body,
RequestOptions? requestOptions = null,
CancellationToken cancellationToken = default)
```

#### Parameters

| Name | In | Required | Contract type | Description |
| --- | --- | --- | --- | --- |
| `projectId` | `path` | yes | `OpaqueId` | Opaque identity; authorize organization/project and verify every parent relation before lookup or mutation. Knowledge of an ID grants no access. |
| `Idempotency-Key` | `header` | yes | `string` | Required on every mutation. Scope = organization + project + operation; fingerprint includes raw body bytes, media type, resource and If-Match. Same key/content replays the same receipt; changed content is 409. Retained for 86400 seconds; reconcile an ambiguous outcome within retention before retry, never silently allocate a new key. |
| `X-CSRF-Token` | `header` | yes | `string` | Session-bound synchronizer token from the trusted BFF bootstrap, independent of the unreadable session cookie. Validate same-origin Origin and CSRF before mutation. This contract does not implement or provision the session/CSRF exchange. |
| `displayName` | `query` | yes | `string` | Escaped display text only, never used as a filename or storage key; included in idempotency fingerprint. |
| `body` | body | yes | `any` |  |

#### Responses

| Status | Description | Content |
| --- | --- | --- |
| `202` | Accepted for asynchronous processing; not a completed effect. | application/json (SourceRevision) |
| `401` | unauthenticated; recovery=authenticate. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `403` | forbidden; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `404` | not_found; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `406` | not_acceptable; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `409` | idempotency_conflict/reconcile_idempotency, binding_conflict/refresh_snapshot, or retry_requires_reconciliation/refresh_snapshot. No additional effect. | application/json (allOf&lt;ApiError, object&gt;) |
| `413` | request_too_large; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `415` | unsupported_media_type; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `422` | invalid_request, source_not_valid or cursor_invalid; never reinterpret an invalid source as ready. | application/json (allOf&lt;ApiError, object&gt;) |
| `429` | rate_limited; recovery=retry_same_request. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `500` | internal_error; recovery=reconcile_idempotency. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `503` | configuration_required/configure_authority when approved JackAI exchange or product configuration is absent; authority_unavailable/retry_same_request on outage/timeout. No fallback identity, cached authorization or synthetic data. | application/json (allOf&lt;ApiError, object&gt;) |

#### Canonical required-input example

```json
{
  "body": "example",
  "content_type": "application/json",
  "parameters": [
    {
      "in": "path",
      "name": "projectId",
      "value": "example"
    },
    {
      "in": "header",
      "name": "Idempotency-Key",
      "value": "AAAAAAAAAAAAAAAA"
    },
    {
      "in": "header",
      "name": "X-CSRF-Token",
      "value": "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA"
    },
    {
      "in": "query",
      "name": "displayName",
      "value": "example"
    }
  ]
}
```

#### SDK call example

Example status: `generated`  
```csharp
var result = await client.Sources.CreateRevisionAsync(projectId: "example", Idempotency-Key: "AAAAAAAAAAAAAAAA", X-CSRF-Token: "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA", displayName: "example", body: "example");
```

<!-- omniwrap-api-operation-end eyJvcGVyYXRpb25faWQiOiJjcmVhdGVTb3VyY2VSZXZpc2lvbiJ9 -->

<!-- omniwrap-api-operation eyJleGFtcGxlX3JlYXNvbiI6bnVsbCwiZXhhbXBsZV9zdGF0dXMiOiJnZW5lcmF0ZWQiLCJtZW1iZXIiOiJTb3VyY2VzLkdldFJldmlzaW9uQXN5bmMiLCJtZXRob2QiOiJHRVQiLCJvcGVyYXRpb25faWQiOiJnZXRTb3VyY2VSZXZpc2lvbiIsInBhdGgiOiIvdjEvcHJvamVjdHMve3Byb2plY3RJZH0vc291cmNlLXJldmlzaW9ucy97cmV2aXNpb25JZH0iLCJyZXNvdXJjZSI6IlNvdXJjZXMiLCJzZWN0aW9uX3NoYTI1NiI6IjcwZjA5YWQ5MjY1ZDY0NDkwMmUxODc1YTBiZGVmNjU0MmZjYjQ2ZmJmNGI1NTgwNThhY2E1ZmQ3M2YwNTQ0ODciLCJzaWduYXR1cmVfc2hhMjU2IjoiMmU0OGMyZmZlMzZhZGM2N2U3ODgyMTMzYmRhMWIwYWUxN2VkMGUzZGI4NDFmNjRlYzM5ZGNmN2Q5MDc1MzZiNCJ9 -->
### `client.Sources.GetRevisionAsync`

Operation ID: `getSourceRevision`  
Route: `GET` `/v1/projects/{projectId}/source-revisions/{revisionId}`  
Equivalent member: `client.GetSourceRevisionAsync`  

#### Signature

```csharp
public Task<SourceRevision> GetRevisionAsync(
string projectId,
string revisionId,
RequestOptions? requestOptions = null,
CancellationToken cancellationToken = default)
```

#### Parameters

| Name | In | Required | Contract type | Description |
| --- | --- | --- | --- | --- |
| `projectId` | `path` | yes | `OpaqueId` | Opaque identity; authorize organization/project and verify every parent relation before lookup or mutation. Knowledge of an ID grants no access. |
| `revisionId` | `path` | yes | `OpaqueId` | Opaque identity; authorize organization/project and verify every parent relation before lookup or mutation. Knowledge of an ID grants no access. |

#### Responses

| Status | Description | Content |
| --- | --- | --- |
| `200` | Authorized resource snapshot. | application/json (SourceRevision) |
| `401` | unauthenticated; recovery=authenticate. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `403` | forbidden; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `404` | not_found; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `406` | not_acceptable; recovery=none. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `422` | invalid_request, source_not_valid or cursor_invalid; never reinterpret an invalid source as ready. | application/json (allOf&lt;ApiError, object&gt;) |
| `429` | rate_limited; recovery=retry_same_request. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `500` | internal_error; recovery=reconcile_idempotency. See the normative error mapping and operation retry policy. | application/json (allOf&lt;ApiError, object&gt;) |
| `503` | configuration_required/configure_authority when approved JackAI exchange or product configuration is absent; authority_unavailable/retry_same_request on outage/timeout. No fallback identity, cached authorization or synthetic data. | application/json (allOf&lt;ApiError, object&gt;) |

#### Canonical required-input example

```json
{
  "parameters": [
    {
      "in": "path",
      "name": "projectId",
      "value": "example"
    },
    {
      "in": "path",
      "name": "revisionId",
      "value": "example"
    }
  ]
}
```

#### SDK call example

Example status: `generated`  
```csharp
var result = await client.Sources.GetRevisionAsync(projectId: "example", revisionId: "example");
```

<!-- omniwrap-api-operation-end eyJvcGVyYXRpb25faWQiOiJnZXRTb3VyY2VSZXZpc2lvbiJ9 -->

