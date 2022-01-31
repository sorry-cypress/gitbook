---
description: Get started with a free parallelization using sorry-cypress
---

# Get Started

Let's start by running basic sorry-cypress configuration:

```
docker run -p 1234:1234 agoldis/sorry-cypress-director
```

We've just launched `director` service on [`http://localhost:1234`](http://localhost:1234) - this service coordinates cypress agents and enables free parallelization.

### Install cy2 and cypress

`cy2` is a  tiny [NPM package](https://www.npmjs.com/package/cy2) that changes cypress API server configuration on-the-fly using  environment variable `CYPRESS_API_URL`

`cy2` passes down all the CLI flags to `cypress` , so you can just use `cy2` instead of `cypress` .

```bash
npm install cy2 cypress
export CYPRESS_API_URL="http://localhost:1234/"
npx cy2 run --record --key XXX --parallel --ci-build-id `date +%s`
```

### Running cypress tests in parallel <a href="#running-cypress-tests-in-parallel" id="running-cypress-tests-in-parallel"></a>

Let's open several terminal windows and run `cypress` in each. Make sure you have cypress tests defined in advance (clone [https://github.com/agoldis/sorry-cypress-demo.git](https://github.com/agoldis/sorry-cypress-demo.git) if you don't have any test handy).

```bash
# run in each terminal
CYPRESS_API_URL="http://localhost:1234/" npx cy2 run --parallel --record --key somekey --ci-build-id hello-cypress
```

You'll notice that different instances of cypress agents are running different tests.&#x20;

🎉 We've just finished the basic setup of sorry-cypress and ran our tests in parallel!

{% hint style="info" %}
* Use the same `--ci-build-id` to associate different cypress agents with the same run
* You can run as many [cypress agents](../concepts/parallelization-guide.md) as you want - each  will run a different test suite
* This basic `director` configuration keeps all the test results in-memory. Restarting it wipes all the data
* `--key` and `projectId` do not have any effect for the basic setup
{% endhint %}
