# Edge Routing and Conditions

After a node completes execution, the engine must select which outgoing edge to follow. This selection uses a deterministic 5-step priority system.

## Edge Selection Priority

The engine evaluates outgoing edges in this order, taking the first match:

| Priority | Mechanism | Description |
|----------|-----------|-------------|
| 1 | **Condition match** | Edge whose `condition` expression evaluates to true against the node's outcome |
| 2 | **Preferred label match** | Edge whose `label` matches the outcome's `preferred_label` field (used by human-in-the-loop responses) |
| 3 | **Suggested node IDs** | Edge leading to a node listed in the outcome's `suggested_next` field |
| 4 | **Weight-based priority** | Edge with the highest `weight` attribute value |
| 5 | **Lexical ordering** | Alphabetically first edge by target node name (deterministic fallback) |

If no edge matches at any priority level, the engine follows the failure routing logic described in [retry-and-failure.md](retry-and-failure.md).

## Condition Expressions

Edge conditions are expressions evaluated against the outcome of the preceding node. The grammar supports:

### Comparison Operators

```
status == 'SUCCESS'
retry_count < 3
confidence >= 0.8
error_type != 'TERMINAL'
```

### Logical Operators

```
status == 'SUCCESS' AND tests_passed == true
status == 'FAILURE' OR timeout == true
NOT has_errors
```

### Key Path Access

Conditions can reference nested values in the outcome or context:

```
outcome.status == 'SUCCESS'
context.test_results.pass_rate > 0.95
```

### String Matching

```
error_message CONTAINS 'timeout'
file_path STARTS_WITH 'src/'
```

See [../05-reference/condition-expressions.md](../05-reference/condition-expressions.md) for the full grammar specification.

## Edge Attributes

| Attribute | Type | Default | Purpose |
|-----------|------|---------|---------|
| `condition` | string | (none) | Expression evaluated against outcome |
| `label` | string | (none) | Display label; also used for accelerator key matching |
| `weight` | integer | 0 | Priority for weight-based selection (higher = preferred) |
| `fidelity` | string | graph default | Context fidelity mode for this transition |

## Label and Accelerator Keys

Edge labels serve double duty — they're display text AND interaction accelerators for `wait.human` nodes:

```dot
approve -> deploy [label="[Y] Yes, deploy"]
approve -> cancel [label="[N] No, cancel"]
```

The engine extracts accelerator keys from `[X]` patterns in labels. When a human responds, the engine normalizes the response and matches against these keys. See [human-in-the-loop.md](human-in-the-loop.md).

## Multiple Matching Edges

When multiple edges have conditions that evaluate to true, the engine selects based on:

1. **Specificity**: More specific conditions (more operators, deeper key paths) are preferred
2. **Weight**: Higher weight breaks ties
3. **Declaration order**: First declared edge in the DOT file wins if all else is equal

## Unconditional Edges

Edges without a `condition` attribute are unconditional — they're always eligible for selection. An unconditional edge with a high `weight` acts as a preferred default path.

## See Also

- [execution-engine.md](execution-engine.md) — How edge selection fits into the traversal loop
- [state-and-context.md](state-and-context.md) — The Outcome model that conditions evaluate against
- [human-in-the-loop.md](human-in-the-loop.md) — How labels drive human interaction
- [../05-reference/condition-expressions.md](../05-reference/condition-expressions.md) — Full condition grammar
