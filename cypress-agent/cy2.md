# cy2

`cy2` is an experimental, tiny \(just 4KB\) [NPM package](https://www.npmjs.com/package/cy2) that changes cypress API server configuration on-the-fly using  environment variable `CYPRESS_API_URL`

The commands passes down to cypress all the CLI flags, so you can just use it instead of `cypress` command.

```bash
$ npm install -g cy2

$ export CYPRESS_API_URL="https://sorry.yourdomain.com/"
$ cy2 run --record --key XXX --parallel --ci-build-id `date +%s`
```

* Running the command above will invoke your default cypress version with the flags you've provided. It will also modify the internal cypress configuration to use a different API URL.
* If no URL provided, it will use the default `cypress` package value.

