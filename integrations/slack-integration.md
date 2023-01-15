# Slack Integration

Sorry-cypress integrates with [Slack Webhooks API](https://api.slack.com/messaging/webhooks). Use the web dashboard Project Settings to add or edit Slack Integration. Also you can filter messages by event type, by test suite result and by branch. 

![](../.gitbook/assets/screenshot-2021-04-15-at-12.02.29.png)

Here's an example of Slack message posted by sorry-cypress:

![](../.gitbook/assets/screenshot-2021-04-15-at-12.32.28.png)


If you are using the in-memory director and do not have a dashboard you can add hooks to your project via a HTTP `POST` to the `/hooks` route of your director..
Ensure your "projectId" matches that in your cypress.json or cypress.config.js that you wish to add hooks to. Note this will replace all the hooks for the given projectId.

```
//Example POST body to localhost:1234/hooks

{
  "projectId": "test",
  "hooks": [
    {
      "hookId": "1",
      "url": "http://localhost:3005",
      "username": "user",
      "hookEvents": [
        "INSTANCE_FINISH",
        "RUN_FINISH"
      ],
      "hookType": "SLACK_HOOK",
      "slackResultFilter": "ONLY_FAILED"
      "slackBranchFilter": ["main"]
    }
  ]
}

```


