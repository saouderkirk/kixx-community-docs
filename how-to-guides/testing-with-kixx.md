# Testing with Kixx

**Official Documentation Reference**: Based on the [Kixx Test Framework](https://github.com/kixx-framework/kixx/blob/main/docs/kixx-test-framework.md) and [Assertion Library](https://github.com/kixx-framework/kixx/blob/main/docs/assertion-library.md) documentation.

## Overview

Kixx provides a built-in test framework with Sinon integration for unit testing JavaScript applications.

## Test Structure

Tests are organized using `describe()` blocks with `before()`, `after()`, and `it()` functions.

```javascript
import { describe, before, after, it } from "kixx-test";
import { assert, assertEqual } from "kixx-assert";

describe("User Authentication", ({ before, after, it }) => {
    let user;

    before(() => {
        user = createTestUser();
    });

    after(() => {
        cleanupTestUser(user);
    });

    it("should authenticate valid credentials", () => {
        const result = authenticate(user.email, user.password);
        assert(result.success);
    });

    it("should reject invalid credentials", () => {
        const result = authenticate(user.email, "wrong-password");
        assertFalsy(result.success);
    });
});
```

## Best Practices

### Do NOT Nest describe() Blocks

**From official docs**: "Do not nest describe() blocks. Although possible to do so in the Kixx Test frameworks, nested describe blocks become confusing."

```javascript
// ❌ WRONG: Nested describe blocks
describe("User", ({ it }) => {
    describe("Authentication", ({ it }) => {  // Don't do this!
        it("should work", () => {});
    });
});

// ✅ CORRECT: Separate describe blocks
describe("User Authentication", ({ it }) => {
    it("should authenticate valid credentials", () => {});
});

describe("User Profile", ({ it }) => {
    it("should update profile", () => {});
});
```

### Create Discrete Blocks for Each Behavior

Each logical code branch or behavior should have its own describe block.

```javascript
describe("Order Processing - Valid Orders", ({ it }) => {
    it("should calculate total correctly", () => {});
    it("should apply discounts", () => {});
});

describe("Order Processing - Invalid Orders", ({ it }) => {
    it("should reject empty cart", () => {});
    it("should reject invalid payment", () => {});
});
```

### Error Testing with Try-Catch

Use try-catch sequences within `it()` blocks rather than framework-specific error handlers.

```javascript
it("should throw ValidationError for invalid input", () => {
    let error;

    try {
        validateInput(null);
    } catch (err) {
        error = err;
    }

    assert(error);
    assertEqual("ValidationError", error.name);
    assertEqual("INVALID_INPUT", error.code);
});
```

### Assert Properties, Not instanceof

Check individual error properties rather than using `instanceof` checks.

```javascript
// ❌ WRONG: Using instanceof
assert(error instanceof ValidationError);

// ✅ CORRECT: Check properties
assertEqual("ValidationError", error.name);
assertEqual("INVALID_INPUT", error.code);
assertEqual(400, error.status);
```

## Kixx Assert Library

The assertion library provides comprehensive validation functions.

### Basic Assertions

```javascript
import {
    assert,
    assertFalsy,
    assertEqual,
    assertNotEqual,
    assertDefined,
    assertUndefined
} from "kixx-assert";

// Truthiness
assert(value);           // Value is truthy
assertFalsy(value);      // Value is falsy

// Equality (uses === with special handling for NaN and Date)
assertEqual(expected, actual);
assertNotEqual(expected, actual);

// Existence
assertDefined(value);    // Value is not undefined
assertUndefined(value);  // Value is undefined
```

### Type Assertions

```javascript
import {
    assertNonEmptyString,
    assertNumberNotNaN,
    assertArray,
    assertBoolean,
    assertFunction,
    assertValidDate
} from "kixx-assert";

assertNonEmptyString(value);   // Non-empty string
assertNumberNotNaN(value);     // Number, not NaN
assertArray(value);            // Array.isArray()
assertBoolean(value);          // Boolean type
assertFunction(value);         // Function type
assertValidDate(value);        // Valid Date object
```

### Pattern Matching

```javascript
import { assertMatches, assertNotMatches } from "kixx-assert";

// Tests against RegExp, substring inclusion, or equality
assertMatches(/^user_/, userId);
assertMatches("@example.com", email);
assertNotMatches("admin", userRole);
```

### Numeric Comparisons

```javascript
import { assertGreaterThan, assertLessThan } from "kixx-assert";

assertGreaterThan(0, count);        // count > 0
assertLessThan(100, percentage);    // percentage < 100
```

### No Deep Equality

**From official docs**: "The Kixx Assert library intentionally omits deep equality testing. Compare objects by reference when possible, or check individual properties explicitly."

```javascript
// ❌ WRONG: Expecting deep equality
assertEqual(expectedObject, actualObject);

// ✅ CORRECT: Compare by reference or check properties
assertEqual(expectedObject, actualObject);  // Same reference
assertEqual("John", user.firstName);
assertEqual("Doe", user.lastName);
assertEqual(30, user.age);
```

## Sinon Integration

### Spies

Spies record function call information without modifying behavior.

```javascript
import sinon from "sinon";

describe("Logger", ({ before, after, it }) => {
    let consoleSpy;

    before(() => {
        consoleSpy = sinon.spy(console, "log");
    });

    after(() => {
        sinon.restore();  // Always clean up!
    });

    it("should log messages", () => {
        logger.info("Test message");

        assertEqual(1, consoleSpy.callCount);  // Prefer .callCount
        assert(consoleSpy.calledWith("Test message"));
    });
});
```

### Stubs

Stubs provide preprogrammed behavior for testing specific code paths.

```javascript
describe("API Client", ({ before, after, it }) => {
    let fetchStub;

    before(() => {
        fetchStub = sinon.stub(global, "fetch");
    });

    after(() => {
        sinon.restore();
    });

    it("should handle successful response", async () => {
        fetchStub.resolves({
            ok: true,
            json: () => Promise.resolve({ data: "test" })
        });

        const result = await apiClient.get("/users");
        assertEqual("test", result.data);
    });

    it("should handle network errors", async () => {
        fetchStub.rejects(new Error("Network error"));

        let error;
        try {
            await apiClient.get("/users");
        } catch (err) {
            error = err;
        }

        assert(error);
        assertEqual("Network error", error.message);
    });
});
```

### Stub Methods

```javascript
stub.returns(value);           // Return value synchronously
stub.resolves(value);          // Return Promise.resolve(value)
stub.throws(error);            // Throw error synchronously
stub.rejects(error);           // Return Promise.reject(error)
stub.callsFake(fn);            // Call custom function
```

### Call Counting

**From official docs**: "We prefer to use .callCount rather than methods like .calledOnce or calledTwice."

```javascript
// ✅ PREFERRED
assertEqual(1, spy.callCount);
assertEqual(3, spy.callCount);

// ❌ LESS PREFERRED
assert(spy.calledOnce);
assert(spy.calledThrice);
```

### Always Clean Up

Always call `sinon.restore()` in `after()` blocks to prevent test state pollution.

```javascript
after(() => {
    sinon.restore();
});
```

## Running Tests

```bash
# Run all tests
npx kixx test

# Run specific test file
npx kixx test tests/user.test.js

# Run with verbose output
npx kixx test --verbose
```

## Test File Organization

```
project/
├── lib/
│   └── user.js
├── tests/
│   ├── user.test.js
│   ├── auth.test.js
│   └── api.test.js
└── package.json
```

## Summary

1. **One describe block per behavior** - Don't nest describe blocks
2. **Use try-catch for errors** - Check properties, not instanceof
3. **Prefer .callCount** - Over .calledOnce, .calledTwice
4. **Always sinon.restore()** - Clean up in after() blocks
5. **No deep equality** - Check individual properties
6. **Setup in before()** - Configure spies and stubs before assertions

---

**Official Documentation**:
- [Kixx Test Framework](https://github.com/kixx-framework/kixx/blob/main/docs/kixx-test-framework.md)
- [Kixx Assert Library](https://github.com/kixx-framework/kixx/blob/main/docs/assertion-library.md)
