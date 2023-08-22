---
description: Cypress Test Statuses - detailed guide and explanation
---

# Test Status

### Possible Cypress Tests Statuses

A cypress test can be in one of the following states:

* <mark style="color:blue;">**Passed**</mark> - a test successfully completed all attempts without any exceptions or errors during its execution
* <mark style="color:red;">**Failed**</mark> - a test that triggered an exception or one of its assertions failed for all of its attempts
* **Ignored / Pending** - a test was excluded by a developer, e.g. `it.skip()` and was excluded by cypress runner
* <mark style="color:orange;">**Skipped**</mark> - a test was not excluded by a developer, but wasn't run by cypress runner due to an error&#x20;
* <mark style="background-color:purple;">**Flaky**</mark> - a test that passed after a few failing attempts

### Passing Tests

A passed test was successfully executed by cypress runner, including all `before` and `beforeEach` expressions. During its execution, the test runner didn't encounter any timeouts, exceptions or failed assertions.&#x20;

Please note, for tests with multiple attempts, if the last attempt was successful, the whole test will be marked as "Passed" but "Flaky". See [flaky-tests.md](flaky-tests.md "mention") for details.&#x20;

### Failing Tests

A failing test is a test that has either triggered an exception, failed assertion or a timeout during the execution of `before`, `beforeEach` or the test body for each of its attempts.

By default, when a test fails, Cypress will take a screenshot and a video recording capturing the failure. Both will be shown in the test details page.

### **Ignored / Pending Tests**

`Ignored/pending` tests are tests that were:

* explicitly marked by a developer to be excluded, e.g. `it.skip(),` `describe.skip()` or `xit()`
* a test that is not implemented - i.e. a test that has no "body". For example, `it('nothing is here')`
* a test suite that was filtered out in [test/suite configuration](https://docs.cypress.io/guides/core-concepts/writing-and-organizing-tests#Test-Configuration), for example:

```javascript
describe('Skip in Chrome', { browser: '!chrome' }, () => {/* ... */})
```

### Skipped Tests

A skipped test is a test that **was supposed to run but has been skipped** because of a runtime error.

One of the most common examples is a situation where there's a crash in `beforeEach.` After unsuccessfully running the first test and recognizing an error in `beforeEach,` Cypress runner "skips" the rest of the tests because they would fail due to the same error.