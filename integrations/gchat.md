# GChat Integration

Sorry-cypress integrates with [GChat Webhooks](https://developers.google.com/chat/how-tos/webhooks).
Use the web dashboard Project Settings to add or edit GChat Integration.

![](../.gitbook/assets/gchat-hook.png)

Here's an example of a GChat message posted by sorry-cypress:

![](../.gitbook/assets/gchat-hook-message.png)

If you are using the in-memory director and do not have a dashboard you can add hooks to your project via a HTTP `POST` to the `/hooks` route of your director. 
Ensure your "projectId" matches that in your cypress.json or cypress.config.js that you wish to add hooks to. Note this will replace all the hooks for the given projectId.

```
//Example POST body to localhost:1234/hooks

{
  "projectId": "test",
  "hooks": [
    {
      "hookId": "1",
      "url": "http://localhost:3005",
      "hookEvents": [
        "INSTANCE_FINISH",
        "RUN_FINISH"
      ],
      "hookType": "GCHAT_HOOK"
    }
  ]
}

```