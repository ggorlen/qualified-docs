---
summary: Help for the Qualified Testing Framework under JavaScript
tags:
  - javascript
  - js
  - testing
---

# Qualified Testing Framework

Qualified provides a custom testing framework for JavaScript. However, we recommend using [Mocha BDD](/reference/languages/javascript/mocha-bdd) and [Mocha TDD](/reference/languages/javascript/mocha-tdd) instead.

:::tip
When taking a challenge, the test framework is already selected for you.  You can determine which framework you are using by looking in the upper-right of the _Sample Tests_ code area.
:::

## `Test` Object

The Test object provides the testing functionality needed to validate a challenge's requirements, or to write your own tests if you are taking a challenge. It is a frozen object and cannot be modified.

# Pass/Fail methods

## `Test.expect(passed, msg)`

Core assertion method that all other methods build off of. `msg` argument is optional. If it is not provided then a generic message will be used. Best practice is to provide your own message.

Pass/Fail status will be written to the output stream.

## `Test.assertEquals(actual, expected, msg)`

Checks that the actual value equals (`===`) the expected value. A useful message will be displayed for both pass and fail outcomes. The `msg` argument is optional. If given it will be displayed in addition to the typical message used.

## `Test.assertNotEquals(actual, unexpected, msg)`

Checks that the actual value does not equal (`!==`) the unexpected value. A useful message will be displayed for both pass and fail outcomes. The `msg` argument is optional. If given it will be displayed in addition to the typical message used.

## `Test.assertSimilar(actual, expected, msg)`

Checks that the actual value equals (`===`) the expected value. `Test.inspect` is used to wrap the values being tested, allowing for similar values to be considered the same. A useful message will be displayed for both pass and fail outcomes. The `msg` argument is optional. If given it will be displayed in addition to the typical message used.

## `Test.assertNotSimilar(actual, unexpected, msg)`

Checks that the actual value does not equal (`!==`) the unexpected value. `Test.inspect` is used to wrap the values being tested, allowing for similar values to be considered the same. A useful message will be displayed for both pass and fail outcomes. The `msg` argument is optional. If given it will be displayed in addition to the typical message used.

## `Test.expectError(msg, fn)`

Useful for testing that an error was expected to happen. `msg` is optional but best practice is to provide one.

## `Test.expectNoError(msg, fn)`

Useful for testing that an error was not expected to happen. `msg` is optional but best practice is to provide one.

# Spec Methods

## `describe(msg, fn)`

Top level method for describing/grouping a set of tests. Globally aliased as `describe`.

```js
describe("Foo", () => {

});

// or

Test.describe("Foo", () => {

});
```

## `it(msg, fn)`

Used in conjunction with `describe` to group related sets of tests in a spec. Globally aliased as `it`.

```js
describe("Foo", () => {
  it ("should be defined", () => {
    Test.expect(this.Foo, "Foo is not defined")
  });
});
```

## `before(fn)`

Any callbacks sent to this method will be called before each `it` spec is run.

```js
// this is a contrived example
describe("Foo", () => {
  let a = 0;

  // called before each spec is run
  before(() => {
    a = 0;
  });

  it("should should do something", () => {
    a++;
    Test.assertEquals(a, 1)
  });

  it("should do something else", () => {
    a += 2;
    Test.assertEquals(a, 2)
  });
});
```

# Helper Methods

## `Test.callCount(methodName) → Integer`

Returns the number of times a `Test` method has been called. Useful when creating challenge that need to test the usage of `Test` methods. Also useful when using `Test` helper methods are disallowed within the challenge solution. You can simply test if any disallowed calls have been made.

## `Test.inspect(object) → String`

Returns a string representation of the object. For simple objects `toString()` will be used. For complex objects a JSON representation will be given.

## `Test.randomize(array) → Array`

Shuffles the contents of an array.  This is useful for generated randomly ordered tests, to reduce cheating.

## `Test.randomNumber() → Integer`

Returns a random integer.

## `Test.randomToken() → String`

Returns a random string of characters.

## `Test.sample(array) → object`

Returns a single, randomly chosen item from an array.
