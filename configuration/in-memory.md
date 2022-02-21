---
description: Sorry-cypress Director basic setup instructions
---

# Basic Setup

The basic sorry-cypress setup:

* enables tests parallelization with [grouping support](https://docs.cypress.io/guides/guides/parallelization.html#Grouping-test-runs)
* does not require any database
* does not require any storage
* does not support integrations \(web hooks, Slack and GitHub integration\)
* keeps all the data in-memory

This setup might be useful for simple workflows when you **do** need parallelization but **don't need** the overhead of maintaining and paying for the infrastructure required to keep and browse tests results.

One could even create such a service on-demand every CI run and terminate at the end of CI process.

In order to start the director in the default, basic setup just run

```text
docker run agoldis/sorry-cypress-director
```

By default the service starts on port `1234`. Point cypress agents to use the newly launched service and see the tests running in parallel.

Behind the scene director service uses in-memory execution driver and can be explicitly set to basic mode by setting environment variables

```text
EXECUTION_DRIVER="../execution/in-memory"
SCREENSHOTS_DRIVER="../screenshots/dummy.driver"
PORT=1234
```

{% hint style="info" %}
To achieve parallelization for the same CI run, make sure that all CI machine are using the same `sorry-cypress-director` service and use the same `--ci-build-id` flag
{% endhint %}

{% hint style="info" %}
`--key` flag has no effect - all keys are accepted for the basic setup. Same for cypress `projectId`
{% endhint %}



