# vibe-coding-test-strategies

How to test code an AI wrote. Different from testing human-written code because AI fails differently.

## Why this exists

Standard testing advice ("write tests first," "aim for high coverage") assumes humans wrote the code. AI-generated code has distinct failure patterns, hallucinated APIs, fake comprehensiveness, plausible-but-wrong logic, that need different test strategies to catch.

This is a guide to those strategies.

## Strategies

| Strategy | When to use |
|---|---|
| [characterization-tests-first](./strategies/characterization-tests-first.md) | Before letting AI refactor or modify untested code |
| [mutation-test-the-tests](./strategies/mutation-test-the-tests.md) | Verifying AI-generated tests actually test |
| [property-based-for-pure-functions](./strategies/property-based-for-pure-functions.md) | Testing functions where examples miss edge cases |
| [adversarial-input-suite](./strategies/adversarial-input-suite.md) | Catching what the agent didn't think to test |
| [run-the-mocks-against-reality](./strategies/run-the-mocks-against-reality.md) | When AI mocks don't match real APIs |
| [golden-tests-for-known-output](./strategies/golden-tests-for-known-output.md) | Capturing known-good outputs as regression bait |
| [contract-tests-at-boundaries](./strategies/contract-tests-at-boundaries.md) | Validating AI's understanding of API contracts |
| [smoke-tests-against-real-deps](./strategies/smoke-tests-against-real-deps.md) | Beyond unit tests: does it actually work? |
| [coverage-as-tripwire-not-target](./strategies/coverage-as-tripwire-not-target.md) | Using coverage correctly with AI code |

## How to read

Each strategy file describes:

- The technique
- When AI code makes this technique especially valuable
- How to set it up
- What it catches that standard testing misses

## Contribute

PRs welcome. See [CONTRIBUTING.md](./CONTRIBUTING.md). Strategies must be ones you've actually used and seen catch real bugs.

## More

Part of a catalog of single-file browser tools and plain-language references, all MIT licensed and dependency-free: [0xelitesystem.github.io](https://0xelitesystem.github.io/). Built by [elitesystem.ai](https://elitesystem.ai).

## License

MIT.

## Related

- [vibe-coding-anti-patterns](https://github.com/0xelitesystem/vibe-coding-anti-patterns) - the failures these strategies catch
- [ai-generated-code-review-rubrics](https://github.com/0xelitesystem/ai-generated-code-review-rubrics) - rubrics that complement testing
- [ai-coding-prompt-recipes](https://github.com/0xelitesystem/ai-coding-prompt-recipes) - prompts that prevent test-related issues
