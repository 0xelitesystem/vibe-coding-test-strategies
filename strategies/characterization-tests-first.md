# Characterization tests first

## The technique

Before letting AI refactor or modify code that has no tests, write characterization tests. These are tests that capture the code's CURRENT behavior, including any bugs, so you can detect when the AI changes that behavior unintentionally.

## When AI code makes this especially valuable

AI agents will "fix" things they consider bugs while refactoring. If you didn't characterize the original behavior, you don't know whether the change was an intended fix, a different bug, or a correct refactor.

## How to set up

1. Identify the function / module to be modified
2. For each public entry point, capture inputs and outputs from real (or representative) data
3. Codify those input/output pairs as tests
4. Run them, they should all pass against the original code
5. Now let AI refactor

If the tests still pass after AI changes, behavior is preserved.
If they fail, AI changed behavior, review whether it's intentional.

## Example

```python
# Before AI refactor, capture current behavior
def test_parse_log_line_characterization():
    assert parse_log_line("INFO 2024-01-01 hello") == ("INFO", "2024-01-01", "hello")
    assert parse_log_line("INFO   2024-01-01    hello") == ("INFO", "2024-01-01", "hello")  # multi-space
    assert parse_log_line("") == None  # empty
    assert parse_log_line("INVALID") == None  # malformed
    # ... more cases capturing edge behavior
```

If AI's refactor changes any of these outputs, you'll see it immediately.

## What this catches

- AI "improvements" that change behavior
- AI refactors that fix a "bug" you actually depend on
- Subtle off-by-one or null-handling changes

## What this misses

- Cases you didn't think to characterize
- New code paths the AI introduces

So: combine with adversarial input testing for new behavior.
