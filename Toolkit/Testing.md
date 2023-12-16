## Concepts

**Automated testing**
- performed by a testing tool that executes a pre-defined test script and generate results. e.g. Jest
- important part of CI/CD pipeline

#### Common types

- **Unit tests** : test small isolated parts of codebase. Interaction with other units is *mocked* eg. class, methods, etc.
- **Integration tests**: test interaction across *real* unit-tested portions of code like modules, database, etc.
- **Smoke test / sanity test**: done before deep tests. Checks critical software functionality using unit+integration tests
- **Functional tests**: test business requirements of app and verify output.
- **End to End tests**: replicates a user behavior with the software.
- **Performance tests**: evaluate system's performance under different conditions, identify memory leaks, bottlenecks, and scalability issues. Major ways -> *load testing* (to identify breakpoint) and *stress testing* (to identify behaviour) under heavy load.
- **Regression tests**: ensure new changes don't break exisiting functionalities.
- **Acceptance tests**: testing against predefined acceptance criteria set by stakeholders.
- **Usability tests**: ensure software is user-friendly and improve UX
- **Exploratory tests**: identify defects and other issues without following a predefined test plan
- **Fuzz tests**: test with random or unexpected inputs to identify how it responds to unexpected data.
- **Compatibility Testing**: ensuring that software works seamlessly on different platforms.

#### Levels of tests

- **Whitebox tests**: test logic of app
- **Blackbox tests**: test behaviour of app, not logic
- **Greybox tests**: combine both
- **Alpha tests:** test in controlled environment
- **Beta/UAT tests:** test in real environment by selected end users


## JEST

Setup
```shell
npm i -D jest   #then install babel
touch jest.config.json 
mkdir test     #place test files here
...create config/test
npm test #to run tests

#support typescript
npm i -D jest typescript ts-jest @types/jest  #babel's ts has issues
```

support esm and ts
```json
//jest.config.json
{
	"transform": {},
	"preset": "ts-jest",
	"testEnvironment": "node"
}

//package.json
{
	"type": "module",
	"scripts": {
    "test": "node --experimental-vm-modules node_modules/jest/bin/jest.js"
    },
}

//tsconfig.json  ... only for TS
{
  "exclude": [
    "node_modules",
    "test", 
  ],
  "compilerOptions": {
    "esModuleInterop": true
  }
}
```

tsconfig options
```json
"testPathIgnorePatterns": ["test/demo.*"]  //exclude tests
```

### Unit test with Jest

Basic test
```js
//your JS or TS file
export default function sum(a, b) {
  return a + b;
}

//demo.test.js or demo.test.ts
import sum from './sum';   //no .js or .ts
test('1+2 should be 3', () => {
  expect(sum(1, 2)).toBe(3);
  //more expects...
});
```


Rules
1. only write code enough to make a failing unit test pass.
2. Only write a unit test sufficient to fail

Matchers
```js
.toBe(val) // Object.is
.toEqual(val) // if val is obj
.toStrictEqual(val) // flag undefined props & type mismatches

.not.toBe(val) 
.toBeTruthy(), etc
.toMatch(/regex/)
.toHaveProperty('prop', optional_val)

//error match
expect(() => func()).toThrow(val) //can be Error, 'str', /reg/
```

