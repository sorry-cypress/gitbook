---
description: One-liners to easily change cypress configuration
---

# CLI One Liners - Deprecated

{% hint style="danger" %}
**Please note**

Cypress 11 and 12+ deprecated agent configuration files. Please make sure to use the latest version of cy2 package. See more at [https://currents.dev/readme/guides/cypress-compatibility](https://currents.dev/readme/guides/cypress-compatibility)
{% endhint %}

Use this CLI one-liner to change cypress configuration for all installed versions of cypress

```bash
sed -i -e 's|api_url:.*$|api_url: "https://sorry-cypress-demo-director.herokuapp.com/"|g' /*/.cache/Cypress/*/Cypress/resources/app/packages/server/config/app.yml
```

Or for Windows:

```bash
ls $env:LOCALAPPDATA/Cypress/Cache -Recurse -Filter app.yml |
% { (Get-Content $_ -Raw) -replace "https://api.cypress.io/", "https://sorry-cypress-demo-director.herokuapp.com/" | Out-File $_ }
```
