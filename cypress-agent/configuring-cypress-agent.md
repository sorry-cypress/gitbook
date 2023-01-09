---
description: Changing cypress agent configuration
---

# Agent Configuration - Deprecated

{% hint style="danger" %}
**Please note**

Cypress 11 and 12+ deprecated agent configuration files. Please make sure to use the latest version of cy2 package. See more at [https://currents.dev/readme/guides/cypress-compatibility](https://currents.dev/readme/guides/cypress-compatibility)
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

