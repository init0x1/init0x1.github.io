---
layout: post
title: Unit Testing With Jasmine
date: 2023-03-17
categories: [Backend, jasmine, unit testing]
---

# jasmine
Jasmine is a popular testing framework that is often used in Test Driven Development (TDD) workflows. TDD is an approach to software development where tests are written before the actual code is written. 

## Configuring Jasmine to work with typeScript

## Install Jasmine
To install Jasmine run:
```
npm i jasmine 
```

## Add a reporter for outputting Jasmine results to the terminal
```
npm i jasmine-spec-reporter
```

## Add type definitions for Jasmine 
```
npm i --save-dev @types/jasmine
```

## Add Testing Scripts
Find the scripts object in the `package.json` and add the following to run jasmine:
```
"jasmine": "jasmine"
```

## Set Up the File Structure
In the root directory of the project, create a folder named `spec`.
In the `spec` folder, create a folder named `support`.
In the `support` folder, create a file named `jasmine.json` to hold the primary configurations for Jasmine.
In the `src` folder, add a folder named `tests`.
In `tests` add a file named `indexSpec.ts` to hold the tests for code in the `index.js` file.
In the `tests` folder, add another folder named `helpers`.
In `helpers`, add a file named `reporter.ts`. This will be the primary configuration for your spec reporter.

### File Structure
```
├── node_modules
├── spec
│      └── support
│           └── jasmine.json
├── src
│     ├──  tests
│     │     ├── helpers
│     │     │      └── reporter.ts
│     │     └── indexSpec.ts
│     └── index.ts
├── package-lock.json
├── package.json
└── tsconfig.json
```

## Best Practices For File Naming
When creating files for tests, a best practice is to name the `.ts` file the same as the `.js` file to be tested with `Spec` appended to the end. The more tests needed to be run, the more test files will need to be created. Be sure to follow this best practice to keep track of the test file that contains the tests for each `.js` file.


 In `reporter.ts`, add the following code from the `jasmine-spec-reporter` TypeScript support documentation to configure the reporter to display Jasmine results to your terminal application. These are default settings and can be adjusted to meet your specific needs. The documentation on GitHub provides more options available.

```ts
import {DisplayProcessor, SpecReporter, StacktraceOption} from "jasmine-spec-reporter";
import SuiteInfo = jasmine.SuiteInfo;

class CustomProcessor extends DisplayProcessor {
    public displayJasmineStarted(info: SuiteInfo, log: string): string {
        return `${log}`;
    }
}

jasmine.getEnv().clearReporters();
jasmine.getEnv().addReporter(new SpecReporter({
    spec: {
        displayStacktrace: StacktraceOption.NONE
    },
    customProcessors: [CustomProcessor],
}));
```

In the `jasmine.json` add the following configurations for a basic Jasmine configuration:

```json
{
    "spec_dir": "dist/tests",
    "spec_files": [
        "**/*[sS]pec.js"
    ],
    "helpers": [
        "helpers/**/*.js"
    ],
    "stopSpecOnExpectationFailure": false,
    "random": false
}
```

In the `tsconfig.json` file, add `"spec"` to the list of folders that we want to exclude.

```
  "exclude": ["node_modules", "./dist", "spec"]
```

## Writing a Basic Test

We'll start with a simple test:

`index.ts`

```typescript
const myFunc = (num: number): number => {
  return num * num;
};

export default myFunc;
```

We can write a simple test for the function:

`indexSpec.ts`

```typescript
import myFunc from '../index';

it('expect myFunc(5) to equal 25', () => {
  expect(myFunc(5)).toEqual(25);
});
```

To test this we'll need to first run the build script and then the test script:

```
npm run jasmine
npm run test
```

Or we can combine the two into one script in our `package.json` file:

```json
"test": "npm run build && npm run jasmine"
```

## Suites and Specs 


A suite is a group of related specs. You can use the `describe` function to define a suite. The `describe` function takes two arguments: a string that describes the suite, and a function that contains the specs for the suite. Here's an example:

```javascript
describe('My Suite', () => {
  // Specs go here
});
```

In this example, we're defining a suite called "My Suite". We can then add specs to this suite by writing them inside the function that we pass to `describe`.

A spec is a single test case. You can use the `it` function to define a spec. The `it` function takes two arguments: a string that describes the spec, and a function that contains the code for the spec. Here's an example:

```javascript
it('should do something', () => {
  // Test code goes here
});
```

In this example, we're defining a spec that should do something. We can then write the code for the spec inside the function that we pass to `it`.

Here's an example of how you might use suites and specs to test a simple function:

```javascript
function add(a, b) {
  return a + b;
}

describe('add function', () => {
  it('should add two numbers', () => {
    const result = add(2, 3);
    expect(result).toEqual(5);
  });

  it('should return NaN if either argument is not a number', () => {
    const result1 = add(2, 'hello');
    const result2 = add('world', 3);
    expect(result1).toBeNaN();
    expect(result2).toBeNaN();
  });
});
```

In this example, we're testing a function called `add`. We've defined a suite called "add function" using the `describe` function, and we've added two specs to this suite using the `it` function. The first spec checks that the function correctly adds two numbers, and the second spec checks that the function returns `NaN` if either argument is not a number.



## comparison method in Jasmine:

- `.toBe()`: This method checks if the tested object is the same object as the expected value. For example:

```javascript
const expected = { name: 'Abdo', age: 21 };
const actual = { name: 'Abdo', age: 21 };

expect(actual).toBe(expected); // This will fail because actual and expected are not the same object.
```

- `.toEqual()`: This method checks if the tested object has the same content as the expected value. For example:

```javascript
const expected = { name: 'Abdo', age: 21 };
const actual = { name: 'Abdo', age: 21 };

expect(actual).toEqual(expected); // This will pass because actual and expected have the same content.
```

- `.toMatch()`: This method checks if the tested string matches a regular expression. For example:

```javascript
const message = 'Hello, world!';

expect(message).toMatch(/world/); // This will pass because the message string contains the word "world".
```

- `.toBeDefined()`: This method checks if the tested object is defined. For example:

```javascript
const value = 5;

expect(value).toBeDefined(); // This will pass because value is defined.
```

- `.toBeUndefined()`: This method checks if the tested object is undefined. For example:

```javascript
let value;

expect(value).toBeUndefined(); // This will pass because value is undefined.
```

- `.toBeNull()`: This method checks if the tested object is null. For example:

```javascript
const value = null;

expect(value).toBeNull(); // This will pass because value is null.
```

- `.toBeTruthy()`: This method checks if the tested object is truthy (i.e. not false, 0, "", null, or undefined). For example:

```javascript
const value = 'hello';

expect(value).toBeTruthy(); // This will pass because value is a non-empty string, which is truthy.
```

- `.toBeFalsy()`: This method checks if the tested object is falsy (i.e. false, 0, "", null, or undefined). For example:

```javascript
const value = 0;

expect(value).toBeFalsy(); // This will pass because value is 0, which is falsy.
```

- `.toContain()`: This method checks if the tested array or string contains a specific value. For example:

```javascript
const fruits = ['apple', 'banana', 'orange'];

expect(fruits).toContain('banana'); // This will pass because the fruits array contains the value "banana".
```

## Numerical Matchers

- `.toBeCloseTo(expected value, precision)`: This method passes if a value is within a specified precision of the expected value. Precision is optional and represents the number of decimal points to check (defaults to 2). For example:

```javascript
const value = 1.23456789;

expect(value).toBeCloseTo(1.23); // This will pass because value is within 2 decimal points of 1.23.
expect(value).toBeCloseTo(1.234567, 6); // This will pass because value is within 6 decimal points of 1.234567.
```

- `.toBeGreaterThan(expected value)`: This method passes if the tested value is greater than the expected value. For example:

```javascript
const value = 5;

expect(value).toBeGreaterThan(3); // This will pass because value is greater than 3.
```

- `.toBeLessThan(expected value)`: This method passes if the tested value is less than the expected value. For example:

```javascript
const value = 5;

expect(value).toBeLessThan(8); // This will pass because value is less than 8.
```

- `.toBeGreaterThanOrEqual(expected value)`: This method passes if the tested value is greater than or equal to the expected value. For example:

```javascript
const value = 5;

expect(value).toBeGreaterThanOrEqual(3); // This will pass because value is greater than or equal to 3.
expect(value).toBeGreaterThanOrEqual(5); // This will pass because value is equal to 5.
```

- `.toBeLessThanOrEqual(expected value)`: This method passes if the tested value is less than or equal to the expected value. For example:

```javascript
const value = 5;

expect(value).toBeLessThanOrEqual(8); // This will pass because value is less than or equal to 8.
expect(value).toBeLessThanOrEqual(5); // This will pass because value is equal to 5.
```

## Exceptions

- `.toThrow(expected value)`: This method passes if the tested function throws an exception. The `expected value` argument is optional and checks that the exception thrown matches the expected value. For example:

```javascript
function throwError() {
  throw new Error('Something went wrong');
}

expect(throwError).toThrow(); // This will pass because throwError() throws an exception.
expect(throwError).toThrow(Error); // This will pass because the exception thrown is an instance of the Error class.
expect(throwError).toThrow('Something went wrong'); // This will pass because the exception message matches the expected value.
```

- `.toThrowError(expected value, expected message)`: This method passes if the tested function throws an exception with the specified error type and message. The `expected value` argument is optional and checks that the exception thrown is an instance of the expected value. The `expected message` argument is also optional and checks that the exception message matches the expected message. For example:

```javascript
function throwError() {
  throw new TypeError('Invalid argument');
}

expect(throwError).toThrowError(); // This will pass because throwError() throws an exception.
expect(throwError).toThrowError(TypeError); // This will pass because the exception thrown is an instance of the TypeError class.
expect(throwError).toThrowError(TypeError, 'Invalid argument'); // This will pass because the exception type and message match the expected values.
```

Note that you can use either `.toThrow()` or `.toThrowError()` depending on your testing needs. The latter provides more specific error checking capabilities, while the former is more general.

## Testing Asynchronous Code

- Using `async/await` syntax 

Use `async` and `await`: If your asynchronous code uses promises, you can use `async` and `await` to make your tests more readable. `async` tells Jasmine that your test function is asynchronous, and `await` waits for a promise to resolve before continuing. For example:


```javascript
describe('Async function', () => {
  it('should return the correct result', async () => {
    const result = await someAsyncFunction();
    expect(result).toEqual(expectedValue);
  });
});
```

In this example, we're using the `async` keyword to tell Jasmine that our test function is asynchronous. We're also using the `await` keyword to wait for the `someAsyncFunction` function to return a value before continuing with our test.

- Using `promise syntax`

you can use promises to handle the asynchronous behavior. Here's an example of how you can use promise syntax with Jasmine:

```javascript
describe('Async function', () => {
  it('should return the correct result', () => {
    return someAsyncFunction().then(result => {
      expect(result).toEqual(expectedValue);
    });
  });
});
```

In this example, we're using the `return` keyword to return a promise from our test function. We're then using the `then` method to chain a callback function that will run when the promise is resolved. Inside this callback function, we're making our assertions using the `expect` function.

- Testing promise `resolution and rejection with ES6 Promise Matchers Library`

```javascript
describe('Async function', () => {
  it('should resolve with the correct result', () => {
    const promise = someAsyncFunction();
    return expect(promise).toBeResolvedWith(expectedValue);
  });

  it('should reject with the correct error', () => {
    const promise = someAsyncFunctionThatRejects();
    return expect(promise).toBeRejectedWith(expectedError);
  });
});
```

In the first example, we're using the `toBeResolvedWith` matcher to test that the promise returned by `someAsyncFunction` resolves with the expected value. If the promise resolves with a different value, the test will fail.

In the second example, we're using the `toBeRejectedWith` matcher to test that the promise returned by `someAsyncFunctionThatRejects` rejects with the expected error. If the promise rejects with a different error or doesn't reject at all, the test will fail.

## Endpoint Testing


### Benefits of Endpoint Testing
- Confirms that the server is working.
- Confirms that endpoints are configured properly.
- More efficient than manual testing.

### Adding a Framework for Endpoint Testing
Endpoint testing is not native to Jasmine and requires a third-party framework, like Supertest to test the status of responses from servers.

### Setting Up Endpoint Testing
- Install Supertest as a dependency.
  
 ```
 npm i supertest
 ``` 
-  Add type definition to allow the code to compile without TypeScript errors.

```
 npm i --save-dev @types/supertest. 
```
- Import SuperTest in the spec file.

```ts
import supertest from 'supertest';
import app from '../index';

const request = supertest(app);
describe('Test endpoint responses', () => {
    it('gets the api endpoint', async (done) => {
        const response = await request.get('/api');
        expect(response.status).toBe(200);
        done();
    }
)});
```

## Performing Tasks Before and After Tests

ou can use the `beforeEach` and `afterEach` functions to perform setup and teardown tasks before and after each test case in a suite. You can also use the `beforeAll` and `afterAll` functions to perform setup and teardown tasks once before and after all test cases in a suite.

Here's an example of using `beforeEach` and `afterEach`:

```ts
describe('MyFunction', function() {
  let value;

  beforeEach(function() {
    value = 0;
  });

  afterEach(function() {
    value = null;
  });

  it('should increment the value', function() {
    value++;
    expect(value).toEqual(1);
  });

  it('should decrement the value', function() {
    value--;
    expect(value).toEqual(-1);
  });
});
```

In this example, the `beforeEach` function sets the initial value of `value` to `0` before each test case, and the `afterEach` function sets the value of `value` to `null` after each test case. The two test cases increment and decrement value, respectively, and make assertions about its expected value.

Here's an example of using `beforeAll` and `afterAll`:

```ts
describe('MyFunction', function() {
  let value;

  beforeAll(function() {
    value = 0;
  });

  afterAll(function() {
    value = null;
  });

  it('should increment the value', function() {
    value++;
    expect(value).toEqual(1);
  });

  it('should decrement the value', function() {
    value--;
    expect(value).toEqual(0);
  });
});
```
In this example, the `beforeAll` function sets the initial value of `value` to `0` once before all test cases, and the `afterAll` function sets the value of `value` to `null` once after all test cases. The two test cases increment and decrement value, respectively, and make assertions about its expected value.


## Handling More Than One Suite

When you have multiple test suites in your Jasmine test suite, you can use the `beforeEach`, `afterEach`, `beforeAll`, and `afterAll` functions at different levels of the suite hierarchy to control setup and teardown tasks.

Here's an example:

```ts
describe('MyFunction', function() {
  let value;

  beforeAll(function() {
    console.log('Before All - outer suite');
  });

  afterAll(function() {
    console.log('After All - outer suite');
  });

  beforeEach(function() {
    console.log('Before Each - outer suite');
    value = 0;
  });

  afterEach(function() {
    console.log('After Each - outer suite');
    value = null;
  });

  it('should increment the value', function() {
    console.log('Test Case - outer suite');
    value++;
    expect(value).toEqual(1);
  });

  describe('Inner Suite', function() {
    let innerValue;

    beforeAll(function() {
      console.log('Before All - inner suite');
    });

    afterAll(function() {
      console.log('After All - inner suite');
    });

    beforeEach(function() {
      console.log('Before Each - inner suite');
      innerValue = 0;
    });

    afterEach(function() {
      console.log('After Each - inner suite');
      innerValue = null;
    });

    it('should increment the value', function() {
      console.log('Test Case - inner suite');
      innerValue++;
      expect(innerValue).toEqual(1);
    });
  });
});
```
In this example, we have an outer suite and an inner suite. The `beforeAll` and `afterAll` functions at the outer suite level are executed once before and after all test cases in the outer suite and its nested suites. The `beforeEach` and `afterEach` functions at the outer suite level are executed before and after each test case in the outer suite and its nested suites.

Similarly, the `beforeAll`, `afterAll`, `beforeEach`, and `afterEach` functions at the inner suite level are executed at the corresponding levels for the inner suite and its test cases.



## Skipping or Specifying Tests

Sometimes, you may want to skip or focus on certain tests in your Jasmine test suite. Jasmine provides two functions for this: `xit` and `fit`.

`xit` allows you to skip a test case, while `fit` allows you to focus on a test case, meaning only that test case will run and all other test cases will be skipped.

Here's an example:
```ts
describe('MyFunction', function() {
  let value;

  beforeEach(function() {
    value = 0;
  });

  it('should increment the value', function() {
    value++;
    expect(value).toEqual(1);
  });

  xit('should decrement the value', function() {
    value--;
    expect(value).toEqual(-1);
  });

  fit('should double the value', function() {
    value *= 2;
    expect(value).toEqual(0);
  });
});
```
In this example, `we have three test cases`. The first test case increments the value variable and expects it to equal 1. The second test case decrements the value variable and expects it to equal -1. However, this test case is skipped because it is marked with `xit`. The third test case doubles the value variable and expects it to equal 0. This test case is marked with `fit`, which means it is focused on, so only this test case will run and the other two test cases will be skipped.
