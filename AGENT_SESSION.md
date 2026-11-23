# Native .NET SDK sessions

Install the exact generated NuGet archive and retain its independently verified
SHA-256. Import `OmniWrap.Sdk.Agentic`; configure the ordinary SDK `ApiClient` with its API
URL and credentials. The session invokes that client in process, without an LLM
dependency, Python bridge or hosted service.

Implement `IAgentProvider` with `Identity`, `Version` equal to
`omniwrap.agent-provider/v1`, `ProposeAsync` and `CloseAsync`. A proposal carries
reported token/cost usage and either a list of calls or one final JSON value.
`AgentSession.Discovery` and installed `agent-session.json` bind operation IDs to
the actual SDK methods and input schemas. `AgentSession.Propose*` helpers use the
SDK's native parameter types; direct `AgentCall` values remain strictly validated.

Construct `AgentSessionOptions` with the exact archive hash, an explicit operation
allowlist, an `AgentCheckpoint(path, externalKey)` with a key of at least 32 bytes,
all nine positive budgets (`turns`, `model_calls`, `tool_calls`, `tokens`, `cost`,
`elapsed_seconds`, `concurrency`, `input_bytes`, `output_bytes`), a `PersistResult`
projection and an `ApproveWrite(operationId, argumentsHash, cancellationToken)`
callback. Return true only for specifically authorized writes. Every HTTP mutation
and declared write effect requires approval, even when its HTTP verb is GET.

The application supplies the variables named below: `configuredApiClient`,
`provider`, `archiveSha256`, `allowedOperationIds`, `checkpointPath`, `externalKey`,
`projectResult`, `approveWrite`, `input` and `cancellationToken`. The projection has
type `Func<string, object?, object?>`; approval has type
`Func<string, string, CancellationToken, Task<bool>>`. For example:

```csharp
using System.Text.Json;
using OmniWrap.Sdk.Agentic;

var limits = new Dictionary<string, double> {
    ["turns"] = 8, ["model_calls"] = 8, ["tool_calls"] = 20,
    ["tokens"] = 8000, ["cost"] = 1, ["elapsed_seconds"] = 60,
    ["concurrency"] = 2, ["input_bytes"] = 1048576, ["output_bytes"] = 1048576
};
var options = new AgentSessionOptions(
    archiveSha256, allowedOperationIds, limits,
    new AgentCheckpoint(checkpointPath, externalKey), projectResult, approveWrite);
var session = new AgentSession(configuredApiClient, provider, options);
JsonElement result = await session.RunAsync(input, cancellationToken);
```

Call `await session.RunAsync(jsonInput, cancellationToken)`. Calls are validated as
a complete batch before effects, run within the concurrency bound, and produce
receipts in proposal order. Native SDK retries are disabled and buffered responses
use the smaller of the caller's configured limit and the remaining output budget.
Projection removes caller-known secrets before result persistence; no general
secret detection is claimed. Provider reports are trusted and callbacks must honor
cancellation. Uncooperative callbacks cannot be forcibly stopped in-process;
cleanup timeout is explicit. This is not a sandbox or a hosted model evaluation.

Checkpoints authenticate source, archive, provider, registry, allowlist and budgets
with HMAC, chain receipts, and refuse concurrent/stale writers. A persistent wall
deadline prevents resetting time through resume. Keep the checkpoint and `.started`
guard together. Missing/corrupt state is refused; an abandoned `.lock` needs
operator recovery after proving its writer stopped. Power-loss and whole-directory
loss recovery require independent operational qualification.

A completed session resumes without another API call. An interrupted batch remains
in doubt. Inspect the external service, then use `session.Reconcile(callId,
knownResult)` to acknowledge each verified result without repeating HTTP effects.
`AgentSessionException.Code` is `agent_session_refused` or `agent_session_in_doubt`;
`Retryable` is false. Native SDK failures remain local exceptions or an inner cause,
never checkpoint messages. Retry only after explicit reconciliation.

Finite JSON and safe integers, local schema references and supported constraints
are checked before calls. The complete input schema graph must be qualified,
including references and optional/composed branches: a branch the validator cannot
evaluate is never treated as a mismatch. Regex-constrained values, header/cookie arguments from
the model and stream/upload operations are refused in this session profile; their
ordinary SDK APIs remain available. Test malformed batches, budget and permission
failures, actual native calls, cancellation, byte bounds, authenticated recovery and
mutation reconciliation against your API before use with a live provider.
