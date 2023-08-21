---
description: Guide to Cypress Flaky Tests
---

# Flaky Tests

### What is flaky cypress test?

Flaky cypress test is a test that did not succeed from the first attempt. The build will fail only occasionally: One time it will pass, another time fail, the next time pass again, without any changes to the build having been made. Flaky tests are marked with a special badge on run, spec and individual test level.

### How to activate flaky tests detection?

Flaky tests are automatically activated for all cypress tests with [retries](https://docs.cypress.io/guides/guides/test-retries#How-It-Works) enabled. When a test has retries enabled and doesn't not pass from the first attempt, it will be marked as flaky.&#x20;
