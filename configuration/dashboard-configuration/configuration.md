# Configuration

`PORT=8080`

Dashboard will listen on that port



`GRAPHQL_SCHEMA_URL="http://localhost:4000"`

The publicly accessible URL of API service. GraphQL client will use it to pull schema definitions and issue queries.



`CI_URL=Link name,https://your.ci.service/project/{project_id}/build/{build_id}`

Set optional environment variable `CI_URL` to add a link to your CI tool.

* `Link name` is the link name, for example: `Travis CI`, `Drone CI`, `Circle CI`.
* `{project_id}` is a template tag for cypress project name - will be injected dynamically.
* `{build_id}` is a template tag for ID passed to cypress via `--ci-build-id` parameter - will be injected dynamically.

Example:

![](../../.gitbook/assets/ci_url.png)



