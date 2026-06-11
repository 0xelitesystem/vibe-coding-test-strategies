# Mutation test the tests

## The technique

Mutation testing deliberately introduces small changes (mutations) to your code and runs the test suite. If a mutation passes the tests, your tests don't actually catch that mutation, they're not sensitive enough.

## When AI code makes this especially valuable

AI generates tests that pass. The training signal didn't strongly distinguish "passes when correct" from "passes regardless." Mutation testing reveals whether tests would catch real bugs.

## How to set up

Tools by language:

- **JavaScript/TypeScript**: Stryker (`@stryker-mutator/core`)
- **Python**: mutmut, cosmic-ray
- **Java**: Pitest
- **Go**: go-mutesting
- **Rust**: mutagen
- **Ruby**: mutant

Install, configure to target the code AI modified, run.

## Reading the output

Each tool produces a "mutation score", % of mutations caught by tests. Plus a list of survived mutants (mutations that passed all tests).

For AI-generated code, the survived mutants are what you care about. They show specific gaps where the tests don't actually verify behavior.

## Example finding

```diff
// Original
function isAdult(age) {
  return age >= 18;
}

// Mutation: changed >= to >
function isAdult(age) {
  return age > 18;
}
```

If your tests still pass, you have no test for `age == 18`. The AI's tests probably tested 17 and 21 but not the boundary.

## What this catches

- Empty assertions
- Tests that exercise code without verifying outcomes
- Missing edge case tests
- Tautological assertions (`expect(value).toBeDefined()` style)

## What this misses

- Logic errors that aren't mutation-equivalent
- Issues outside the mutation surface (e.g. missing tests entirely)

## Cost

Mutation testing is slow (multiplies test suite runtime by N mutations). Run it:

- Once after a major AI refactor
- Periodically in CI (e.g. nightly, not per-PR)
- On critical modules where test quality matters most
