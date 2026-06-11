# Golden tests for known output

## The technique

Save expected outputs as files. Compare actual output to the saved file. When intentional changes happen, regenerate the golden file.

Aliases: snapshot tests, approval tests, characterization tests (with file output).

## When AI code makes this especially valuable

AI-generated formatters, parsers, generators, and transformers benefit. Golden tests capture exactly what was working, so any AI change that affects output triggers a review.

## How to set up

```typescript
test('renders user profile', () => {
  const output = renderProfile(USER_FIXTURE);
  expect(output).toMatchSnapshot();
});
```

The first run creates `__snapshots__/test.ts.snap` with the output. Subsequent runs compare to that file.

When AI changes the renderer:
- Test fails, showing diff
- You review the diff
- If intentional, run with `--updateSnapshot` to regenerate
- Commit the new golden file alongside the code change

## What goes in golden tests

Good fits:
- HTML / JSX / template output
- JSON / XML serialization
- CLI output (text formatting)
- Generated code (codegen)
- Error message formatting

Bad fits:
- Anything with timestamps / random IDs (use redaction)
- Massive outputs (review fatigue)
- Very volatile output (golden gets updated constantly, defeats the purpose)

## Reviewing golden diffs

When the snapshot changes, READ the diff. Don't just `--updateSnapshot` reflexively.

The point of golden tests is they make output changes visible. Updating without review removes the value.

## Variations

- **Approval testing** (Approvaltests): golden files but with workflow tooling
- **Storybook snapshot tests**: for UI components
- **Schema-anchored tests**: golden + schema validation
- **VCR / pytest-recording**: golden HTTP interactions

## What this catches

- AI changing output format unintentionally
- AI "improving" formatting that breaks downstream consumers
- AI fixing bugs that other code depended on

## What this misses

- Logic errors that don't surface in output
- Intermittent issues
