# Smoke tests against real dependencies

## The technique

Beyond unit tests, run a small set of tests that actually exercise real dependencies, real DB, real HTTP servers, real file system.

## When AI code makes this especially valuable

AI mocks plausibly. Real dependencies behave differently from mocks in ways AI doesn't anticipate (timeouts, partial reads, retries, transactions, eventual consistency).

## How to set up

Tier your tests:

1. **Unit tests**: fast, mocks everything, run on every save
2. **Integration tests**: real local dependencies (Docker test DB, test HTTP server), run on commit
3. **Smoke tests**: real cloud dependencies (test environment), run pre-deploy
4. **End-to-end**: real-world scenario, run nightly or pre-release

Smoke tests are the layer most often skipped. They're the bridge between "tests pass" and "actually works."

## What goes in smoke tests

Pick the highest-value paths:

- Login flow end-to-end
- Place an order end-to-end
- Read-then-write-then-read on real DB
- File upload to real S3
- Send a real (test) email through real SMTP

5-10 smoke tests cover the most-important user flows. They're slow but they're shippable confidence.

## Example smoke test

```typescript
test.smoke('user registration -> login -> read profile', async () => {
  const email = `smoke-${Date.now()}@example.com`;

  // Real registration via real endpoint
  const reg = await fetch('https://test.api/register', {
    method: 'POST',
    body: JSON.stringify({ email, password: 'test123' })
  });
  expect(reg.status).toBe(201);

  // Real login
  const login = await fetch('https://test.api/login', {...});
  const token = (await login.json()).token;

  // Real authenticated read
  const profile = await fetch('https://test.api/profile', {
    headers: { Authorization: `Bearer ${token}` }
  });
  expect((await profile.json()).email).toBe(email);
});
```

## What this catches

- Auth flows with real session handling
- Database transaction behavior (mocks lie about isolation levels)
- Real timeouts / retries
- Configuration mismatches between dev and test environments
- AI assumptions about API behavior that mocks didn't reveal

## What this misses

- Production-only issues (different scale, different load)
- Eventual consistency at production scale

## Cost / speed

Smoke tests are slow. Run:

- Pre-deploy in CI (gating production)
- Nightly (catching environmental drift)
- On-demand when something feels off
