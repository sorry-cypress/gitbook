---
description: Changing cypress agent configuration
---

# Agent Configuration

{% hint style="info" %}
You can easily set cypress API server URL by using NPM[`cy2`](https://www.npmjs.com/package/cy2) package.  [Read more.](cy2.md)
{% endhint %}

Find cypress installation path

```bash
DEBUG=cypress:* cypress version

# here it is
cypress:cli Reading binary package.json from: /Users/john/Library/Caches/Cypress/3.4.1/Cypress.app/Contents/Resources/app/package.json +0ms
```

In my case it is: `/Users/john/Library/Caches/Cypress/6.3.0/Cypress.app/Contents/Resources/app/`

Change the default dashboard URL - use the  `director` service URL

```bash
$ cat /Users/john/Library/Caches/Cypress/3.4.1/Cypress.app/Contents/Resources/app/packages/server/config/app.yml

...
# Replace this with a URL of the alternative dashboard
production:
  # api_url: "https://api.cypress.io/"
  api_url: "http://localhost:1234/"
...
```

{% hint style="info" %}
Make sure to change the configuration on every machine that runs cypress test in your CI environment
{% endhint %}



