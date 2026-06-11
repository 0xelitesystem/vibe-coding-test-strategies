# Adversarial input suite

## The technique

Maintain a per-project library of "hostile" inputs. Run them against new code as part of your test suite or pre-merge checks.

## Categories

- **Empty / null**: empty string, null, undefined, empty array, empty object
- **Boundary numerics**: 0, -1, MAX_INT, MIN_INT, NaN, Infinity, very small floats
- **Long strings**: 1KB, 1MB, 10MB
- **Unicode mayhem**: emoji, RTL text, combining characters, zero-width joiners, mixed scripts
- **Malformed JSON**: missing braces, trailing commas, wrong types, deeply nested
- **Path traversal**: `../`, encoded `..`, absolute paths, Windows paths on Unix and vice versa
- **Injection-shaped**: `'; DROP TABLE`, `<script>`, `${userInput}`, `{{templateInjection}}`
- **Encoding edges**: BOM, mixed encodings, percent-encoded
- **Time edges**: epoch, year 1900, year 2100, leap day, DST transitions, timezone boundaries
- **Floats**: 0.1 + 0.2, very small, very large, denormalized

## When AI code makes this especially valuable

AI tests typical inputs. Adversarial inputs find what AI didn't think to handle. Especially common AI gaps:

- Unicode handling (AI assumes ASCII)
- Time zones (AI assumes UTC or local)
- Empty collections (AI assumes non-empty)
- Very large inputs (AI assumes "reasonable" size)

## How to set up

Create a fixtures file with reusable adversarial inputs:

```python
# tests/fixtures/adversarial.py

EMPTY = ["", None, [], {}, 0, False]

UNICODE = [
    "\U0001f680"  # emoji codepoint,                    # emoji
    "\u65e5\u672c\u8a9e"  # CJK chars,                  # CJK
    "\u05d0" + "\u0300",          # combining char
    "a\u200bb",              # zero-width space
    "\u202eevil.exe",        # RTL override
]

LARGE = [
    "x" * 10_000,
    "x" * 1_000_000,
]

PATH_TRAVERSAL = [
    "../etc/passwd",
    "..\\windows\\system32",
    "%2e%2e%2f",
    "/absolute/path",
]
```

Use these in property tests or as parameterized example tests.

## What this catches

- Unicode handling bugs
- Off-by-one on boundary values
- Missing input validation
- Crashes on hostile but possible input

## How to maintain

When you find a new edge case in production, add it to the fixture file. Over time, the suite becomes a record of every weird input you've seen.
