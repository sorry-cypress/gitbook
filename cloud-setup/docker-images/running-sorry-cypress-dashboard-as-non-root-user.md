# Running Sorry Cypress Dashboard as non-root user

{% hint style="info" %}
Thanks to [@mathpaquette](https://github.com/mathpaquette) for  contributing this guide
{% endhint %}

Standard Nginx docker image requires privileged user to in order run properly, i.e. your user needs to be added to default [docker group](https://docs.docker.com/engine/install/linux-postinstall). On the other hand, in some corporate networks, docker engine has been configured to run under non-root user which requires additional steps to setup.

### Getting config files

First you need to start sorry-cypress dashboard image to extract default configuration files.

```text
docker run -it --user <name|uid>[:<group|gid>] --name dashboard agoldis/sorry-cypress-dashboard:latest /bin/sh
```

Then you need to copy 2 configuration files from running container to your host. Open a new terminal without terminating the previous one.

```text
docker cp dashboard:/etc/nginx/nginx.conf .
docker cp dashboard:/etc/nginx/templates/default.conf.template default.conf
```

Now you can remove the current running container.

```text
docker rm dashboard -f
```

### Adjusting config files

Now it's time to tweak a little bit config files from default image.

#### `nginx.conf`

1. Comment out the user directive:

```text
#user nginx
```

2. Replace all directives that point to `/var/*` to `/tmp` instead:

```text
error_log /tmp/error.log warn;
pid /tmp/nginx.pid;

access_log /tmp/access.log main;
```

3. Add these directives in http section:

`default.config`

You just need to replace all `${}` string patterns to your matching environment variables. Example:

Before

`listen ${PORT} default_server;`

After

`listen 8080 default_server;`

{% hint style="info" %}
Make sure you choose a port &gt; 1024 to be bindable from a non-root user.
{% endhint %}

### Copying config files

Now, let's create a new container with default entry point. Don't worry, initial boot will fail.

```text
docker run -it --user <name|uid>[:<group|gid>] --name dashboard agoldis/sorry-cypress-dashboard:latest
```

Then, copy updated config files:

```text
docker cp nginx.conf dashboard:/etc/nginx
docker cp default.conf dashboard:/etc/nginx/conf.d
```

### Running updated container

Now, you can run sorry-cypress dashboard under unprivileged user, simply but doing:

```text
docker start dashboard
```

You're done!

