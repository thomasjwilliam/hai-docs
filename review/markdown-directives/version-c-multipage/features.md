# Features

Interactive behaviors available on directives through attributes or action buttons.

---

## Commit

The commit action replaces a directive with its finalized content, removing the directive markup. Use it to convert a template into plain output.

### How it works

| Directive type | Result of committing |
|----------------|---------------------|
| `input`, `textarea` | Replaced by the entered value |
| `note`, `div`, `span` | Replaced by inner content only (directive wrapper removed) |
| `button` | Removed entirely |

Nested directives are committed from innermost outward, so all descendant values are resolved before the container is unwrapped.

### Enabling commit

**On a specific directive** — add the `commit` attribute:
```
{{note type="info" commit}}
  Customer: {{input value="Jane Smith"}}
  Order: {{input value="Widget A"}}
{{/note}}
```

After committing:
```
Customer: Jane Smith
Order: Widget A
```

**On all directives in the document** — press `Cmd/Ctrl + Alt + P`.

**Per-field commit** — press `Cmd/Ctrl + P` while a field is focused, or use the `commit` boolean attribute on an `input` or `textarea` to commit automatically on entry.

---

## Copy

Copies the content of a directive to the clipboard as HTML. Content is committed before copying, so form values are resolved.

Enable on a directive with the `copy` attribute:
```
{{div copy}}
  Summary: {{input value="Q4 report"}}
{{/div}}
```

Click the copy button (📋) in the directive's toolbar to copy.

---

## Fold

Collapses a directive to show only its header, expanding again on click. Fold state is visual only — it is not saved to the document.

Enable on `note` or `div` with the `fold` attribute:
```
{{note type="info" title="Background" fold}}
  Long context that readers can collapse to save space.
{{/note}}

{{div shaded fold}}
  ## Attendees
  {{textarea rows="4"}}
{{/div}}
```

---

## Keyboard shortcuts

### Global

| Shortcut | Action |
|----------|--------|
| `Cmd/Ctrl + Alt + P` | Commit all committable directives in the document |
| `Cmd/Ctrl + Alt + Q` | Toggle quick input mode |

### Within an input or textarea

| Shortcut | Action |
|----------|--------|
| `Cmd/Ctrl + Enter` | Exit the field and place cursor after the directive |
| `Cmd/Ctrl + P` | Commit the field value |
| `Cmd/Ctrl + Backspace/Delete` | Remove the directive |

### Navigation

| Shortcut | Action |
|----------|--------|
| `Tab` | Move to the next form element |
| `Shift + Tab` | Move to the previous form element |

Within `checkbox-group` and `radio-group`, `Tab` moves between options and then advances to the next element after the last option.

### Quick input mode

When quick input mode is active (`Cmd/Ctrl + Alt + Q`), committing a field value (`Cmd/Ctrl + P`) automatically moves focus to the next form field. This enables rapid sequential data entry without manual navigation.

---

← [Back to index](./index.md)
