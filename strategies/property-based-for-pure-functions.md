# Property-based testing for pure functions

## The technique

Instead of testing specific input/output examples, define properties (invariants) that should hold for ALL valid inputs. The framework generates inputs to try.

Common properties:

- Reversibility: `decode(encode(x)) == x`
- Idempotence: `f(f(x)) == f(x)`
- Commutativity: `f(a, b) == f(b, a)`
- Bounds: `0 <= percent(x) <= 100`
- Round-trip: parse-then-format produces same input

## When AI code makes this especially valuable

AI tests example inputs. Property tests catch what AI didn't think to enumerate. Especially good for:

- Parsing / serialization (round-trips)
- Math utilities (commutativity, monotonicity)
- Sort / dedupe / merge (size relationships)
- Encoding / decoding

## How to set up

Tools by language:

- **JavaScript/TypeScript**: fast-check
- **Python**: hypothesis
- **Java**: jqwik
- **Go**: testing/quick (stdlib) or rapid
- **Rust**: proptest, quickcheck
- **Ruby**: rantly

## Example

```typescript
import fc from 'fast-check';

test('encode/decode round-trip', () => {
  fc.assert(
    fc.property(fc.string(), (s) => {
      expect(decode(encode(s))).toBe(s);
    })
  );
});
```

The framework will try thousands of strings, including:
- Empty
- Whitespace
- Unicode
- Emoji
- Strings with backslashes, quotes, control characters
- Very long strings
- Strings the AI wouldn't have manually tested

## What this catches

- Edge cases in input space
- Encoding bugs
- Off-by-one in iteration
- Bugs that only manifest at boundary values

## What this misses

- Side effects (property tests work best for pure functions)
- Bugs that require specific structured inputs (use example tests for those)

## Limitations

Property tests don't replace example tests. Use both:

- Example tests document specific behaviors
- Property tests find unknown unknowns
