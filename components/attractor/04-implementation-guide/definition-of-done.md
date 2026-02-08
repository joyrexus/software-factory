# Definition of Done

Consolidated validation checklists from all three specs. An implementation is complete when every item is checked.

## Unified LLM Client

### Core Infrastructure
- [ ] `Client.from_env()` auto-discovers providers from environment variables
- [ ] Programmatic client construction with explicit adapters
- [ ] Provider routing dispatches requests to correct adapter
- [ ] Default provider used when `provider` omitted
- [ ] `ConfigurationError` raised when no provider configured
- [ ] Middleware chain executes in correct order (request: registration, response: reverse)
- [ ] Module-level default client works
- [ ] Model catalog populated with `get_model_info()` and `list_models()`

### Provider Adapters (for each: OpenAI, Anthropic, Gemini)
- [ ] Uses provider's **native API** (not compatibility shim)
- [ ] Authentication works (API key from env or explicit)
- [ ] `complete()` returns correctly populated `Response`
- [ ] `stream()` returns correctly typed `StreamEvent` iterator
- [ ] System messages handled per provider convention
- [ ] All 5 roles translated correctly
- [ ] `provider_options` escape hatch passes through
- [ ] Beta headers supported (especially Anthropic)
- [ ] HTTP errors translated to correct error hierarchy
- [ ] `Retry-After` headers parsed

### Message & Content Model
- [ ] Text-only messages work across all providers
- [ ] Image input (URL, base64, local file) translated correctly
- [ ] Tool call content parts round-trip correctly
- [ ] Thinking blocks preserved with signatures (Anthropic)
- [ ] Multimodal messages (text + images) work

### Generation
- [ ] `generate()` works with prompt and with messages
- [ ] `generate()` rejects both prompt and messages simultaneously
- [ ] `stream()` yields correct event types
- [ ] `generate_object()` returns validated structured output
- [ ] Cancellation and timeouts work

### Reasoning Tokens
- [ ] OpenAI reasoning tokens returned via Responses API
- [ ] `reasoning_effort` passed through to OpenAI
- [ ] Anthropic thinking blocks returned as THINKING content parts
- [ ] Gemini thinking tokens mapped to `reasoning_tokens`

### Prompt Caching
- [ ] OpenAI: automatic, `cache_read_tokens` populated
- [ ] Anthropic: auto-injected `cache_control`, correct beta header
- [ ] Gemini: automatic, `cache_read_tokens` populated
- [ ] Multi-turn session shows >50% cache hits by turn 5+

### Tool Calling
- [ ] Active tools (with handler) trigger automatic execution loops
- [ ] Passive tools (without handler) return calls without looping
- [ ] `max_tool_rounds` respected
- [ ] Parallel tool calls executed concurrently, all results sent together
- [ ] Tool errors sent as error results, not exceptions
- [ ] `ToolChoice` modes translated per provider

### Error Handling & Retry
- [ ] Correct error types for all HTTP status codes
- [ ] Exponential backoff with jitter works
- [ ] `Retry-After` header overrides backoff
- [ ] Non-retryable errors raised immediately
- [ ] Streaming not retried after partial delivery

### Cross-Provider Parity Matrix
For each of (OpenAI, Anthropic, Gemini):
- [ ] Simple text generation
- [ ] Streaming text generation
- [ ] Image input (base64 and URL)
- [ ] Single tool call + execution
- [ ] Multiple parallel tool calls
- [ ] Multi-step tool loop (3+ rounds)
- [ ] Streaming with tool calls
- [ ] Structured output
- [ ] Reasoning/thinking tokens
- [ ] Error handling (401, 429)
- [ ] Usage token accuracy
- [ ] Prompt caching (cache hits on turn 2+)

---

## Coding Agent Loop

### Core Loop
- [ ] LLM call → tool execution → result → repeat cycle works
- [ ] Natural completion (no tool calls) ends the session
- [ ] Round and turn limits enforced
- [ ] Abort signal stops execution
- [ ] Timeout stops execution

### Tools and Execution
- [ ] Provider-specific tool profiles loaded correctly
- [ ] Core tools work (read_file, write_file, run_command, list_directory, search_files)
- [ ] Provider-specific edit tools work (apply_patch for OpenAI, edit_file for Anthropic)
- [ ] Custom tools can be registered
- [ ] ExecutionEnvironment interface properly abstracts I/O
- [ ] Environment variable filtering prevents secret leakage

### Truncation
- [ ] Character-based truncation works
- [ ] Line-based truncation works
- [ ] Head/tail split preserves beginning and end
- [ ] Per-tool limits applied correctly
- [ ] Full output stored in events/artifacts

### Loop Detection
- [ ] 1-call cycles detected
- [ ] 2-call cycles detected
- [ ] 3-call cycles detected
- [ ] Warning injected on first detection
- [ ] Session stops on repeated detection

### Steering and Context
- [ ] Steering messages injected before next LLM call
- [ ] Follow-up messages persist across turns
- [ ] System prompt tiers assembled correctly
- [ ] Project instruction files (AGENTS.md, etc.) loaded

### Subagents
- [ ] Child sessions spawn with independent history
- [ ] Shared filesystem access works
- [ ] Depth limiting enforced
- [ ] Subagent results returned to parent

---

## Attractor Pipeline

### Graph Definition
- [ ] DOT subset parsed correctly (digraph, nodes, edges, attributes, subgraphs)
- [ ] Variable expansion in attributes works
- [ ] Chained edges handled correctly

### Validation
- [ ] Single start and exit node enforced
- [ ] Reachability verified
- [ ] Condition syntax validated
- [ ] Known handlers verified
- [ ] Diagnostics produced with correct severity

### Execution Engine
- [ ] 5-phase lifecycle works (parse → validate → initialize → execute → finalize)
- [ ] Traversal loop executes handlers and selects edges
- [ ] 5-step edge selection priority implemented correctly
- [ ] Error boundaries convert handler exceptions to outcomes

### State and Context
- [ ] Key-value context store works
- [ ] Context updates from handlers merge correctly
- [ ] Built-in keys maintained (_current_node, _run_id, etc.)
- [ ] Context fidelity modes implemented

### Checkpoints
- [ ] Checkpoints saved after each node
- [ ] Resume from checkpoint works
- [ ] Goal gate status preserved
- [ ] Retry counters preserved

### Retry and Failure
- [ ] Retry with backoff works
- [ ] Failure routing priority (fail edge → retry_target → fallback → terminate)
- [ ] Non-retryable errors skip retry
- [ ] Retry counters track correctly

### Goal Gates
- [ ] Pipeline cannot exit with unsatisfied gates
- [ ] Automatic re-routing for unsatisfied gates
- [ ] Gate exhaustion terminates pipeline

### Parallelism
- [ ] Context cloned per branch
- [ ] Branches execute concurrently
- [ ] Join policies (wait_all, first_success, quorum, k_of_n) work
- [ ] Context consolidation merges correctly

### Human-in-the-Loop
- [ ] Interviewer interface works (at least Console + AutoApprove)
- [ ] Edge labels → choices mapping correct
- [ ] Accelerator key matching works
- [ ] Timeout handling works

### Model Stylesheet
- [ ] CSS-like parsing works
- [ ] Specificity rules applied correctly (* < .class < #id)
- [ ] Resolved model propagated to sessions

### Observability
- [ ] Events emitted at lifecycle points
- [ ] Run directory structure populated
- [ ] Artifacts stored per node

---

## Integration Smoke Test

The ultimate validation — an end-to-end pipeline run:

- [ ] Load a multi-node DOT file
- [ ] Execute with codergen nodes making real LLM calls
- [ ] Conditional routing based on outcomes
- [ ] At least one goal gate that requires retry
- [ ] Checkpoint saved and resume tested
- [ ] Final output is a working codebase that passes its tests

## See Also

- [build-sequence.md](build-sequence.md) — Which checklist to use at each phase
- [decision-points.md](decision-points.md) — Decisions that affect what "done" means
