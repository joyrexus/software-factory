# Model Stylesheet

The model stylesheet provides centralized, CSS-like configuration of which LLM models are used across the pipeline. Instead of scattering model assignments across individual nodes, you define rules in a stylesheet.

## CSS-Like Grammar

```css
/* Universal selector — applies to all nodes */
* {
    llm_model: claude-sonnet-4-5-20250929;
    llm_provider: anthropic;
}

/* Class selector — applies to nodes with matching class attribute */
.validation {
    llm_model: claude-opus-4-6;
    reasoning_effort: high;
}

/* ID selector — applies to a specific node by name */
#critical_review {
    llm_model: gpt-5.2;
    llm_provider: openai;
    reasoning_effort: high;
}
```

## Selector Specificity

Following CSS conventions, more specific selectors override less specific ones:

| Selector | Specificity | Example |
|----------|-------------|---------|
| `*` (universal) | Lowest | `* { llm_model: claude-sonnet-4-5-20250929 }` |
| `.class` (class) | Medium | `.validation { llm_model: claude-opus-4-6 }` |
| `#id` (ID) | Highest | `#critical_review { llm_model: gpt-5.2 }` |

When multiple rules match a node, the most specific rule wins. For rules at the same specificity level, the last-declared rule wins (standard CSS cascade).

## Declarations

| Property | Description | Example Values |
|----------|-------------|----------------|
| `llm_model` | Model identifier | `claude-opus-4-6`, `gpt-5.2`, `gemini-2.5-pro` |
| `llm_provider` | Provider name | `anthropic`, `openai`, `gemini` |
| `reasoning_effort` | Reasoning intensity | `low`, `medium`, `high` |
| `max_tokens` | Maximum output tokens | `4096`, `8192` |
| `temperature` | Sampling temperature | `0.0`, `0.7`, `1.0` |

## Node Class Attribute

Nodes reference stylesheet classes via the `class` attribute:

```dot
run_tests [shape=box goal="Execute test suite" class=validation]
check_lint [shape=box goal="Run linter" class=validation]
write_code [shape=box goal="Implement feature" class=generation]
```

Both `run_tests` and `check_lint` would inherit the `.validation` rules.

## Override Precedence

Model configuration resolves in this order (highest priority first):

1. **Node-level attributes**: `llm_model` set directly on the node
2. **ID selector**: `#node_name { ... }` in the stylesheet
3. **Class selector**: `.class_name { ... }` in the stylesheet
4. **Universal selector**: `* { ... }` in the stylesheet
5. **Engine defaults**: Built-in fallback model

## Inline vs External Stylesheet

The stylesheet can be:

- **Inline**: Embedded in the graph's `model_stylesheet` attribute
- **External**: Referenced as a file path in `model_stylesheet`

```dot
// Inline
graph [model_stylesheet="* { llm_model: claude-sonnet-4-5-20250929 } .validation { llm_model: claude-opus-4-6 }"]

// External file reference
graph [model_stylesheet="models.css"]
```

## Why This Matters

Without the stylesheet, model assignment is scattered:

```dot
// Without stylesheet — model config on every node
node_a [shape=box goal="..." llm_model=claude-sonnet-4-5-20250929]
node_b [shape=box goal="..." llm_model=claude-sonnet-4-5-20250929]
node_c [shape=box goal="..." llm_model=claude-opus-4-6]
```

The stylesheet centralizes this, making it easy to:
- Change the default model for all nodes in one place
- Use expensive models only where needed (validation, critical review)
- Experiment with different model assignments without editing the pipeline graph

## See Also

- [graph-definition.md](graph-definition.md) — Node and graph attributes
- [node-types-and-handlers.md](node-types-and-handlers.md) — How codergen uses the resolved model
- [../03-unified-llm-client/client-setup.md](../03-unified-llm-client/client-setup.md) — Model identifiers and provider routing
