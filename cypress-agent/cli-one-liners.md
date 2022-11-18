---
description: One-liners to easily change cypress configuration
---

# CLI One Liners

{% hint style="danger" %}
Cypress 11 deprecated agent configuration files.

See the migration guide [https://currents.dev/readme/guides/cypress-11](https://currents.dev/readme/guides/cypress-11)
{% endhint %}

{% hint style="info" %}
You can easily change cypress API server URL by using NPM[`cy2`](https://www.npmjs.com/package/cy2) package.  [Read more.](cy2.md)
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
