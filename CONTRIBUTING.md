# Contributing a Test Strategy

## What makes a good strategy

A test strategy is a technique that has caught real bugs in AI-generated code. Not a tool recommendation, not generic testing advice.

Each entry must include:

1. **The technique** - what to do, concretely
2. **When AI code makes this especially valuable** - the specific failure mode
3. **How to set up** - tools and example
4. **What this catches** - specific bug categories
5. **What this misses** - boundaries
6. **Cost** - speed/effort tradeoff

## Submission

1. Add a file in `strategies/`
2. Add a row to README.md
3. Open a PR

PR title: `add strategy: name` or `update strategy: name`

## What gets rejected

- Generic testing advice ("write tests")
- Tool advertisements
- Strategies without concrete examples
- Strategies that don't acknowledge cost/limitations
- Strategies copied from blog posts without adapting

## License

By contributing, you agree your contribution is MIT-licensed.
