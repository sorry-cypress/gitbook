---
description: Get started with a free parallelization using sorry-cypress
---

# Get Started

Let's start by running basic sorry-cypress configuration:

```text
docker run agoldis/sorry-cypress-director
```

We've just launched `director` service on [`http://localhost:1234`](http://localhost:1234) - this service coordinates cypress runners and enables free parallelization.

### Re-configuring cypress agent

Now we need to override cypress agents configuration to start using the local `director` service:

```bash
# Let's find cypress runner location
DEBUG=cypress:* cypress version

# Examine the output, note the path
"cypress:cli Reading binary package.json from: /Users/agoldis/Library/Caches/Cypress/6.2.1/Cypress.app/Contents/Resources/app/package.json +0ms"

# Now let's override cypress agent configuration
vim /Users/agoldis/Library/Caches/Cypress/6.2.1/Cypress.app/Contents/Resources/app/packages/server/config/app.yml
production:
  # api_url: "https://api.cypress.io/"
  api_url: "http://localhost:1234/"
```

### Running cypress tests in parallel <a id="running-cypress-tests-in-parallel"></a>

Now let's open several terminal windows and run `cypress` in each. Make sure you have several cypress tests defined in advance \(clone [https://github.com/agoldis/sorry-cypress-demo.git](https://github.com/agoldis/sorry-cypress-demo.git) if you don't have any test handy\).

```bash
# run in each terminal
cypress run --parallel --record --key somekey --ci-build-id hello-cypress
```

You'll notice that different instances of cypress agent are running different tests. 

🎉 We've just finished the basic setup of sorry-cypress and ran our tests in parallel!

{% hint style="info" %}
* Use the same `--ci-build-id` to associate different cypress agents with the same run
* You can run as many cypress agents as you want - each  will run a different test suite
* This basic `director` configuration keeps all the test results in-memory. Restarting it wipes all the data
{% endhint %}



