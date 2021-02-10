# Executable Wrapper

Thanks [@janineahn](https://github.com/janineahn) and [@redaxmedia](https://github.com/redaxmedia) for this contribution!

Instead of changing the `api_url` in the cypress config, it's also possible to reroute the cypress IP in your `/etc/hosts` file.

Sorry-cypress includes an executable helper for this, to use it run `sudo sorry-cypress` \(superuser rights are necessary for editing the hosts file\).

This command use [hostile](https://github.com/feross/hostile) to change your hosts file and will start cypress in a child process. Once Cypress is done or killed the rerouting rule in your hosts file will be deleted.

The command will need the following environment variables:

* `SORRY_CYPRESS_RECORD_KEY`
* `SORRY_CYPRESS_API_IP`
* `SORRY_CYPRESS_BUILD_ID`

```text
sudo SORRY_CYPRESS_BUILD_ID=build-001 SORRY_CYPRESS_RECORD_KEY=whatever SORRY_CYPRESS_API_IP=127.0.0.1 ./bin/sorry-cypress.js <other cypress arguments>
```

{% hint style="warning" %}
Please be aware of the following limitation before using `sorry-cypress.js`

* Only works with `etc/hosts` or `C:/Windows/System32/drivers/etc/hosts` present
* Only works with HTTPS on and port 443 on the target machine
* Has hard coded arguments for the cypress run
* Missing output that CLI started/finished
{% endhint %}



