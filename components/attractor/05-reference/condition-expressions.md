# Condition Expressions

Edge conditions use a small expression grammar evaluated against node outcomes and the pipeline context.

## Grammar

```
expression     = comparison
               | expression AND expression
               | expression OR expression
               | NOT expression
               | '(' expression ')'

comparison     = key_path operator value

key_path       = identifier ('.' identifier)*

operator       = '==' | '!=' | '<' | '<=' | '>' | '>='
               | 'CONTAINS' | 'STARTS_WITH' | 'ENDS_WITH'
               | 'IN'

value          = string_literal | number_literal | boolean_literal | list_literal

string_literal = "'" [^']* "'"
number_literal = [0-9]+ ('.' [0-9]+)?
boolean_literal = 'true' | 'false'
list_literal   = '[' value (',' value)* ']'
```

## Operators

### Comparison Operators

| Operator | Description | Example |
|----------|-------------|---------|
| `==` | Equal | `status == 'SUCCESS'` |
| `!=` | Not equal | `error_type != 'TERMINAL'` |
| `<` | Less than | `retry_count < 3` |
| `<=` | Less than or equal | `confidence <= 0.5` |
| `>` | Greater than | `pass_rate > 0.95` |
| `>=` | Greater than or equal | `coverage >= 80` |

### String Operators

| Operator | Description | Example |
|----------|-------------|---------|
| `CONTAINS` | Substring match | `error_message CONTAINS 'timeout'` |
| `STARTS_WITH` | Prefix match | `file_path STARTS_WITH 'src/'` |
| `ENDS_WITH` | Suffix match | `file_path ENDS_WITH '.py'` |

### Collection Operators

| Operator | Description | Example |
|----------|-------------|---------|
| `IN` | Membership test | `status IN ['SUCCESS', 'PARTIAL']` |

### Logical Operators

| Operator | Description | Example |
|----------|-------------|---------|
| `AND` | Logical conjunction | `status == 'SUCCESS' AND tests_passed == true` |
| `OR` | Logical disjunction | `status == 'FAILURE' OR timeout == true` |
| `NOT` | Logical negation | `NOT has_errors` |

## Operator Precedence

From highest to lowest:
1. `NOT`
2. Comparison operators (`==`, `!=`, `<`, etc.)
3. `AND`
4. `OR`

Use parentheses to override: `(status == 'FAILURE' OR timeout == true) AND retry_count < 3`

## Key Paths

Conditions evaluate against the outcome and context. Key paths allow accessing nested values:

### Direct Keys

```
status                  → outcome.status
retry_count             → context["retry_count"]
```

### Dotted Paths

```
outcome.status          → outcome.status (explicit)
context.test_results    → context["test_results"]
outcome.error.type      → outcome.error.type
```

### Resolution Order

When a bare key (no prefix) is used:
1. Check outcome fields first (`status`, `error`, `preferred_label`)
2. Check context store second
3. If not found, evaluate to `null`

## Semantics

### Null Handling

- Comparing against a missing key: the comparison evaluates to `false`
- `NOT missing_key` evaluates to `true` (NOT null → true)
- `missing_key == null` is not supported — use `NOT missing_key` instead

### Type Coercion

- String `'true'` / `'false'` coerce to booleans in boolean context
- Numbers compare numerically (no string comparison for digits)
- No implicit coercion between strings and numbers

### Short-Circuit Evaluation

- `AND` stops evaluating on the first `false`
- `OR` stops evaluating on the first `true`

## Examples

### Simple Status Check
```
condition="status == 'SUCCESS'"
```

### Compound Condition
```
condition="status == 'FAILURE' AND error.retryable == true AND retry_count < 3"
```

### String Matching
```
condition="error_message CONTAINS 'rate limit'"
```

### Nested Key Path
```
condition="context.test_results.pass_rate >= 0.95"
```

### Membership Test
```
condition="status IN ['SUCCESS', 'PARTIAL']"
```

### Negation
```
condition="NOT has_breaking_changes"
```

## See Also

- [../01-pipeline-orchestrator/edge-routing-and-conditions.md](../01-pipeline-orchestrator/edge-routing-and-conditions.md) — How conditions drive edge selection
- [../01-pipeline-orchestrator/state-and-context.md](../01-pipeline-orchestrator/state-and-context.md) — The Outcome model conditions evaluate against
- [dot-syntax-quick-ref.md](dot-syntax-quick-ref.md) — Where conditions appear in DOT syntax
