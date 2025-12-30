# Chapter 16: Test-Driven Development with Claude

Claude excels when it has a clear target. Tests provide that target. Write the tests first, then let Claude implement code that passes them.

## The TDD Loop with Claude

Traditional TDD:
```
Write failing test → Write code → Test passes → Refactor
```

Claude-assisted TDD:
```
Write failing test → Claude writes code → Test passes → Claude refactors
```

The key insight: Claude performs best when it can iterate against a concrete target.

## Starting with Tests

```
I want to implement a function that validates email addresses.
First, write comprehensive tests for this function. Include:
- Valid email formats
- Invalid formats (missing @, invalid domains, etc.)
- Edge cases (unicode, very long addresses, etc.)

Don't implement the function yet—just the tests.
```

Claude creates the test file. You review.

```
Now implement the function to pass all these tests.
```

Claude writes the implementation. You verify:

```
Run the tests
```

## Anthropic's Recommended Workflow

From Anthropic's best practices:

1. **Write tests with Claude's help**
   ```
   Generate tests for the user authentication module based on 
   these requirements: [requirements]
   ```

2. **Tell Claude not to modify tests**
   ```
   Write code that passes these tests. Do not modify the tests.
   ```

3. **Let Claude iterate**
   ```
   Keep going until all tests pass.
   ```

4. **Verify independently**
   ```
   Use a subagent to verify the implementation isn't 
   overfitting to the tests.
   ```

## Example: Building a Calculator

**Step 1: Tests first**

```
Create a test file for a Calculator class with these methods:
- add(a, b)
- subtract(a, b)
- multiply(a, b)
- divide(a, b)

Include tests for:
- Basic operations with positive numbers
- Negative numbers
- Decimals
- Division by zero (should raise an error)
- Edge cases
```

**Step 2: Run tests (they fail)**

```
Run the tests
```

Output: Tests fail because Calculator doesn't exist yet.

**Step 3: Implement**

```
Now implement the Calculator class to pass all tests.
Do not modify the tests.
```

**Step 4: Verify**

```
Run the tests again
```

All tests pass. The implementation matches the specification.

## Tests as Documentation

Tests describe expected behavior. This helps Claude understand what you want:

```
Here are the existing tests for the OrderService:

@test/order.test.ts

Implement the OrderService to pass these tests.
Focus on the createOrder and cancelOrder methods.
```

The tests tell Claude exactly what the code should do.

## Generating Tests for Existing Code

For legacy code without tests:

```
Generate comprehensive tests for @src/utils/validation.ts

Include:
- Happy path tests for each function
- Edge cases
- Error conditions
- Input validation
```

Claude reads the file, understands the behavior, and creates tests.

## Property-Based Testing

For thorough coverage:

```
Add property-based tests for the sort function.
Use fast-check (or hypothesis for Python).

Test properties like:
- Output length equals input length
- Output is sorted
- Output contains same elements as input
```

Claude generates properties that must hold for all inputs.

## Test-First Refactoring

When refactoring:

1. **Ensure tests exist**
   ```
   First, verify we have tests for all public methods in UserService.
   Generate any missing tests.
   ```

2. **Run tests** (they should pass)

3. **Refactor**
   ```
   Refactor UserService to use the Repository pattern.
   Tests must continue to pass.
   ```

4. **Run tests** (still pass = successful refactor)

## Integration Tests with Claude

```
Create an integration test for the user registration flow:
1. POST to /api/register with valid data
2. Verify user is created in database
3. Verify welcome email is queued
4. POST to /api/login with new credentials
5. Verify JWT is returned
```

Claude creates the full integration test with setup and teardown.

## Mocking with Claude

```
The PaymentService depends on Stripe. Create tests that:
1. Mock the Stripe API
2. Test successful payment processing
3. Test handling of declined cards
4. Test network errors from Stripe
```

Claude creates appropriate mocks and test cases.

## Coverage Analysis

```
Run test coverage and show me which files have less than 80% coverage
```

Then:

```
Generate tests to increase coverage for @src/services/notification.ts
Focus on the untested branches.
```

## Debugging Failing Tests

When tests fail unexpectedly:

```
This test is failing: [paste test output]

1. Explain what the test expects
2. Show what the code actually does
3. Identify the discrepancy
4. Suggest a fix (to the code, not the test)
```

Claude debugs against the test specification.

## Avoiding Test Pollution

When Claude generates tests that are too coupled to implementation:

```
These tests are testing implementation details, not behavior.
Rewrite them to test the public interface only.
The tests should pass even if I change the internal implementation.
```

## Snapshot Testing

For UI or complex output:

```
Add snapshot tests for the UserCard component.
Capture snapshots for:
- Default state
- Loading state
- Error state
- Different user types (admin, regular, guest)
```

Claude sets up snapshot testing appropriate to your framework.

## The TDD Mindset with AI

Human TDD: Tests force you to think about requirements first.

AI-assisted TDD: Tests give Claude an unambiguous target.

The combination is powerful:
- Tests encode your requirements precisely
- Claude implements against those requirements
- Verification is automatic
- Refactoring is safe

## Workflow Integration

### Pre-commit Hook

```bash
# Run tests before allowing commits
claude -p "Run all tests. Exit with status 0 if all pass, 1 if any fail." \
  --allowedTools "Bash(npm test)"
```

### CI Pipeline

```yaml
- name: AI Test Review
  run: |
    claude -p "Review the test coverage for changed files. 
               Suggest any missing test cases." \
      --allowedTools "Read,Grep,Glob" \
      > test-review.md
```

## Summary

Tests + Claude = Reliable development

1. Write tests first (or have Claude write them)
2. Let Claude implement
3. Verify tests pass
4. Refactor safely

Claude excels at hitting targets. Tests are the target.
