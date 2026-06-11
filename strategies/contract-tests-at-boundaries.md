# Contract tests at boundaries

## The technique

At every boundary where data crosses (HTTP API, message queue, file format, RPC), have explicit contract tests that verify the data matches the schema.

## When AI code makes this especially valuable

AI changes API shapes silently, adds fields, renames fields, changes types. Contract tests catch this immediately.

## How to set up

1. Define the contract (OpenAPI, JSON Schema, Protobuf, etc.) once
2. Validate REQUEST data against schema in tests
3. Validate RESPONSE data against schema in tests
4. Run validation on real I/O (in test environment) periodically

## Example: OpenAPI contract test

```typescript
import { OpenAPISchemaValidator } from 'openapi-schema-validator';
import schema from './openapi.json';

const validator = new OpenAPISchemaValidator({ version: 3 });

test('GET /users/:id response matches schema', async () => {
  const response = await fetch('/users/123');
  const data = await response.json();
  const result = validator.validate(data, schema.paths['/users/{id}'].get.responses['200']);
  expect(result.errors).toEqual([]);
});
```

## What this catches

- AI removing a field other code depends on
- AI adding a field that downstream consumers can't handle
- AI changing a type (string to number, optional to required)
- AI shipping a response that doesn't match the docs

## Tools by interface type

- **HTTP**: OpenAPI + dredd / openapi-validator / openapi-typescript
- **GraphQL**: GraphQL Inspector, schema linting
- **gRPC / Protobuf**: buf, protovalidate
- **JSON files**: JSON Schema with ajv / jsonschema (Python)
- **Database**: schema migrations as the contract

## Cross-service contract testing

For service-to-service:

- **Pact**: consumer-driven contracts. Consumer writes its expectations; provider verifies.
- **Spring Cloud Contract**: similar for JVM
- **OpenAPI as boundary**: both sides agree on the schema

## What this misses

- Semantic changes that don't violate schema (renumbering, reordering)
- Behavior changes within the same shape

So: combine with golden tests for behavior.
