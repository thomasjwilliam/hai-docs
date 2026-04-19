# hai.app

Application-level utilities. Access via `hai.app` or destructuring:

```javascript
const { app } = hai;
```

---

## notify(notification)

```typescript
app.notify(notification: {
  message: string
  type: "info" | "success" | "warning" | "error"
  sticky?: boolean
  title?: string
}): void
```

Displays a notification to the user.

### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `message` | `string` | Yes | The notification body text |
| `type` | `"info" \| "success" \| "warning" \| "error"` | Yes | Determines the visual style and icon |
| `sticky` | `boolean` | No | If `true`, the notification stays until manually dismissed. Default: `false` |
| `title` | `string` | No | Optional heading displayed above the message |

### Notification types

| Type | Use for |
|------|---------|
| `"info"` | Neutral information |
| `"success"` | Confirmation that an operation completed |
| `"warning"` | Something requires user attention |
| `"error"` | An operation failed |

### Examples

```javascript
// Minimal — body only
app.notify({ type: "info", message: "Macro started." });

// With title and sticky
app.notify({
  type: "warning",
  title: "Action required",
  message: "Review the highlighted fields before saving.",
  sticky: true
});

// Success after async work
app.notify({ type: "success", title: "Done", message: "All records updated." });

// Error with sticky so it isn't missed
app.notify({
  type: "error",
  title: "Save failed",
  message: "Connection timed out. Try again.",
  sticky: true
});
```

---

← [Back to index](./index.md)
