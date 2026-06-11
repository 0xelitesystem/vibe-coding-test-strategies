# Coverage as tripwire, not target

## The technique

Use code coverage to catch when tests get DELETED or LOSE coverage, not as a number to maximize.

## Why "as a target" is broken

Coverage as a target leads to writing tests that exercise lines without verifying behavior:

```typescript
test('processData runs', () => {
  processData(input);  // executes the code
  // no assertion, but coverage reports 100%
});
```

AI is especially good at producing this kind of test. It looks like effort. It tests nothing.

## "As a tripwire" instead

Set a coverage threshold based on what you have NOW. Not aspirational.

If current coverage is 73%, set the floor at 73%. If a PR drops it, fail the PR.

This catches:
- Test deletion without replacement
- New code added without tests
- Coverage regression as code grows

It doesn't try to push coverage up. The test suite quality is on you, not the coverage tool.

## How to set up

Most coverage tools support thresholds:

```yaml
# jest.config.js
coverageThreshold: {
  global: { lines: 73, branches: 65, functions: 70 }
}
```

When AI changes lower the metric, the PR fails. You either:

1. Add real tests for the new code
2. Lower the threshold (rare; only with explicit reason)
3. Reject the PR

## What this catches

- AI deleting a test along with code
- AI adding code without adding tests
- AI's tests being all "exercise the line" with no real coverage of branches

## What this misses

- Bad tests that maintain coverage but verify nothing
- Mutation-survival rate (use mutation testing for that)

## Use coverage to find untested code, then test it

Coverage's other use: find which parts of your code aren't tested at all. Open the coverage report, find the uncovered functions, write tests for the important ones.

This is one place AI can help: "write tests for these uncovered functions" with the function signatures and current behavior.

## Don't

- Set 100% as a goal
- Treat hitting a coverage number as "done"
- Use coverage tools as an indicator of code quality
- Inflate coverage with throwaway tests to satisfy a metric
