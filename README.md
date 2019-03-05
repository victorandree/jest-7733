# Reproduction of [#7733][issue]

This repository demonstrates that jest does not respect `projects` in the root level `jest.config.js`, but runs "on its own". Run on node v11.6.0.

## Steps to reproduce

Set up the repository (jest@24.1.0):

```shell
npm i
```

Run tests with `--projects` flag and notice how the display name `abc` is shown, suggesting that `a/jest.config.js` is respected.

```shell
$ npx jest --projects a
 PASS   abc  a/index.test.js
  f1
    ✓ should return 100 (5ms)

Test Suites: 1 passed, 1 total
Tests:       1 passed, 1 total
Snapshots:   0 total
Time:        1.243s
Ran all test suites.
```

Then, run tests _without_ any flags and notice that the tests run without any naming:

```shell
$ npx jest
 PASS  a/index.test.js
  f1
    ✓ should return 100 (2ms)

Test Suites: 1 passed, 1 total
Tests:       1 passed, 1 total
Snapshots:   0 total
Time:        0.995s, estimated 1s
Ran all test suites.
```

But, the top level project has this `jest.config.js`:

```javascript
module.exports = {
  projects: ['a'],
};
```

The expectation is that because `projects` is specified in `jest.config.js`, it would work like the command line argument. Since it does not, `npx test` in the root directory will run tests without respecting transformations or other configuration per project (for example, `rootDir` or `ts-jest` configuration).

[issue]: https://github.com/facebook/jest/issues/7733
