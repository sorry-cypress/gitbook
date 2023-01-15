# Webhooks

Sorry-cypress will send a `POST` HTTP request to a URL with a JSON payload.

```typescript
// Payload schema
{
  event: "INSTANCE_FINISH" | "INSTANCE_START" | "RUN_START" | "RUN_FINISH";
  runUrl: string;
  failures: number;
  passes: number;
  skipped: number;
  tests: number;
  pending: number;
  wallClockDurationSeconds: number;
}
```

Use the web dashboard Project Settings to add or edit Generic Webhook Integration

![](../.gitbook/assets/screen-shot-2021-03-11-at-10.57.19-pm.png)

If using the in-memory director and do not have a dashboard you can add webhooks to your project via a `POST` to `/hooks` route to your director.
It will reply with all the Projects. Ensure your "projectId" matches that in your cypress.json or cypress.config.js that you wish to add hooks to. Note this will replace all the hooks for the given projectId.

```
//Example POST body to localhost:1234/hooks

{
  "projectId": "test",
  "hooks": [
    {
      "hookId": "1",
      "url": "http://localhost:3005",
      "hookEvents": "RUN_FINISH",
      "hookType": "GENERIC_HOOK"
    },
    {
      "hookId": "2",
      "url": "http://localhost:3005",
      "hookEvents": "INSTANCE_FINISH",
      "hookType": "GENERIC_HOOK"
    }
  ]
}

```


