# cy2

`cy2` is a tiny [NPM package](https://www.npmjs.com/package/cy2) that changes cypress API server configuration on-the-fly using environment variable `CYPRESS_API_URL`

The command passes down to cypress all the CLI flags, so you can just use it instead of `cypress` .

```bash
$ npm install -g cy2

$ export CYPRESS_API_URL="https://sorry.yourdomain.com/"
$ cy2 run --record --key XXX --parallel --ci-build-id `date +%s`
```

* Running the command above will invoke your default cypress version with the flags you've provided. It will also modify the internal cypress configuration to use a different API URL.
* If no URL is provided, it will use the default `cypress` package configuration
* On Windows set `CYPRESS_API_URL` in the CMD shell with `set CYPRESS_API_URL=https://sorry.yourdomain.com/` (Don't use quotes (`"`) because the Windows CMD takes them literal unlike the Linux shells)
