# BitBucket Integration

Integrating with Bitbucket allows [reporting build status](https://developer.atlassian.com/server/bitbucket/how-tos/updating-build-status-for-commits/) for your commits and pull requests. Use the web dashboard Project Settings to add or edit Bitbucket Integration.

{% hint style="info" %}
**Please note:** Bitbucket app password should have at least "Repositories:write" permissions
{% endhint %}

![Example of Bitbucket Integration](<../.gitbook/assets/Screen Shot 2021-03-11 at 11.14.37 PM.png>)

Sorry-cypress would update build status to failure / success.

![](<../.gitbook/assets/Screen Shot 2021-03-11 at 11.16.28 PM.png>)

If you are using the in-memory director and do not have a dashboard you can add hooks to your project via a HTTP `POST` to the `/hooks` route of your director. Ensure your "projectId" matches that in your cypress.json or cypress.config.js that you wish to add hooks to. Note this will replace all the hooks for the given projectId.

```
//Example POST body to localhost:1234/hooks

{
  "projectId": "test",
  "hooks": [
    {
      "hookId": "1",
      "url": "http://localhost:3005",
      "hookType": "BITBUCKET_STATUS_HOOK",
      "bitbucketUsername": "username",
      "bitbucketToken": "token",
      "bitbucketBuildName": "build"
    }
  ]
}

```
