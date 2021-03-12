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



