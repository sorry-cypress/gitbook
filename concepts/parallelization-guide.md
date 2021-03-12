# Parallelization Explained

Cypress agent is the tool that runs the tests. When the agent is configured to run test in parallel, it tries to connect a remote dashboard to coordinate the tests running order.

Parallelizing cypress tests means running different tests on multiple cypress agents at the same time. The [official Cypress documentation](https://docs.cypress.io/guides/guides/parallelization.html) greatly explains why is it good.

Sorry cypress replaces the original cypress dashboard. You will still need to set up \(and pay for\) a CI environment that runs cypress agents.

![Parallelization Diagram](../.gitbook/assets/parallelization-diagram.png)

When using sorry-cypress you'll need to [override the default configuration](../cypress-agent/configuring-cypress-agent.md) to set an alternative URL for contacting the remote dashboard. 

