# Run the mocks against reality

## The technique

AI-generated mocks often don't match real API behavior. Periodically test that your mocks behave like the real thing.

## When AI code makes this especially valuable

When AI mocks an external API, it imagines what the API returns based on docs or pattern. The imagined response may differ from reality in:

- Field names (snake_case vs camelCase)
- Error response shape
- Status codes for edge cases
- Pagination format
- Date/time format
- Null handling

## How to set up

For each external service you mock, have at least one test that:

1. Uses the real service (in a sandbox / test environment)
2. Captures the actual response
3. Compares it to your mock fixture

Run this test periodically (nightly, not per-PR) and on every API version change.

## Example

```python
@pytest.mark.integration
def test_stripe_charge_response_matches_mock():
    """Verify mocked Stripe response matches actual sandbox response."""
    real_response = stripe.Charge.create(
        amount=100, currency="usd",
        source="tok_visa",  # Stripe test token
        api_key=os.environ["STRIPE_TEST_KEY"]
    )
    mocked_response = mocks.stripe_charge_success()

    # Compare structure (not values)
    assert set(real_response.keys()) == set(mocked_response.keys())
    for key in real_response:
        assert type(real_response[key]) == type(mocked_response[key])
```

## Tools that help

- **Recorded fixtures**: VCR (Ruby), pytest-recording (Python), nock (Node), record real responses, replay in tests
- **Pact**: contract testing between services
- **Schemas**: validate both real and mock responses against the same schema

## What this catches

- AI mocks with wrong field names
- AI mocks that miss fields the real API returns
- AI mocks with wrong error shapes
- API drift between when the mock was written and now

## What this misses

- Subtle differences in real-world response distribution
- Race conditions or stateful API behavior

## Cost

Real API calls take time and may incur costs. Run this strategy as a separate, less-frequent test category, not on every commit.
