# Markdown Directives

Custom syntax elements for embedding interactive UI components — inputs, buttons, and containers — directly in documents.

**Syntax:**
```
{{tagName attribute="value" booleanAttribute}}
{{tagName attribute="value"}}content{{/tagName}}
```

Self-closing directives (`button`, `input`, `option`, `textarea`) have no closing tag.  
Container directives (`checkbox-group`, `datalist`, `note`, `div`, `radio-group`, `select`, `span`) wrap content.

---

## Global Attributes

Available on all directives.

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | `string` | Unique identifier. Use with `hai.doc.getDirectiveById()` in macros |
| `class` | `string` | Space-separated class names. Some names enable built-in behaviors (see below) |
| `aria-label` | `string` | Accessibility label for screen readers |

### Built-in `class` behaviors

| Class name | Behavior |
|------------|----------|
| `commit` | Adds a button that replaces the element with its inner content |
| `copy` | Adds a button that copies content to clipboard (as HTML) |
| `fold` | Adds a collapse/expand toggle |
| `remove` | Adds a button that deletes the element |

Presentation classes: `primary`, `secondary`, `tertiary`, `info`, `tip`, `success`, `warning`, `error`

---

## Form Directives

### button

```
{{button onclick="$MacroName" text="Label"}}
```

Triggers a snippet or macro when clicked.

| Attribute | Required | Default | Description |
|-----------|----------|---------|-------------|
| `onclick` | Yes | — | Trigger name prefixed with `$`. Use `$Name[n]` for nth match when names collide |
| `text` | Yes | — | Button label |
| `color` | No | `primary` | `primary` \| `secondary` \| `accent` \| `info` \| `success` \| `warning` \| `error` |
| `aria-label` | No | Button text | Overrides the implicit label |

```
{{button onclick="$SubmitForm" text="Submit"}}
{{button onclick="$Delete" text="Remove" color="error"}}
```

---

### checkbox-group

```
{{checkbox-group value="a,b"}}
  {{option value="a"}}
  {{option value="b" label="Option B"}}
{{/checkbox-group}}
```

Group of toggleable checkboxes. Use nested [`option`](#option) directives to define items.

| Attribute | Required | Default | Description |
|-----------|----------|---------|-------------|
| `value` | No | — | Comma-separated checked values. Commas within values are auto-escaped |
| `column` | No | — | Boolean. Displays options vertically |

```
{{checkbox-group column}}
  {{option value="email" label="Email"}}
  {{option value="sms" label="SMS"}}
  {{option value="phone" label="Phone"}}
{{/checkbox-group}}
```

---

### datalist

```
{{datalist placeholder="Choose or type..."}}
  {{option value="..."}}
{{/datalist}}
```

Searchable dropdown that also accepts free-text input not in the list.

| Attribute | Required | Default | Description |
|-----------|----------|---------|-------------|
| `placeholder` | No | — | Hint text when empty |
| `value` | No | — | Currently selected or entered value |

```
{{datalist placeholder="Select customer"}}
  {{option value="Acme Corp"}}
  {{option value="Global Industries"}}
{{/datalist}}
```

---

### input

```
{{input type="text" placeholder="..."}}
```

Single-line field for text, dates, or date-times.

| Attribute | Required | Default | Description |
|-----------|----------|---------|-------------|
| `type` | No | `text` | `text` \| `date` \| `datetime-local` |
| `placeholder` | No | — | Hint text when empty |
| `value` | No | — | Current field value |
| `cols` | No | `12` | Width in characters; `auto` = full container width |
| `commit` | No | — | Boolean. Replaces the input with its value immediately on entry |

> [!NOTE]
> Use `type="date"` or `type="datetime-local"` rather than a plain text field for temporal data — they provide a date picker and store values in a standard format.

```
{{input placeholder="Full name" id="name"}}
{{input type="date"}}
{{input type="text" cols="auto" placeholder="Full-width"}}
{{input commit placeholder="Quick entry — press Enter to commit"}}
```

---

### option

```
{{option value="medium" label="Medium"}}
```

A single item within `checkbox-group`, `datalist`, `radio-group`, or `select`. When `label` is omitted, `value` is used as the display text.

| Attribute | Required | Default | Description |
|-----------|----------|---------|-------------|
| `value` | Yes | — | The option's value |
| `label` | No | Same as `value` | Text shown to the user |

---

### radio-group

```
{{radio-group value="selected"}}
  {{option value="..."}}
{{/radio-group}}
```

Single-selection button group. Use nested [`option`](#option) directives.

| Attribute | Required | Default | Description |
|-----------|----------|---------|-------------|
| `value` | No | — | Currently selected value |
| `column` | No | — | Boolean. Stacks options vertically |

```
{{radio-group value="high" column}}
  {{option value="low" label="Low"}}
  {{option value="medium" label="Medium"}}
  {{option value="high" label="High"}}
{{/radio-group}}
```

---

### select

```
{{select placeholder="Choose..."}}
  {{option value="..."}}
{{/select}}
```

Fixed-choice dropdown. Use nested [`option`](#option) directives.

| Attribute | Required | Default | Description |
|-----------|----------|---------|-------------|
| `placeholder` | No | — | Hint text when nothing is selected |
| `value` | No | — | Currently selected value |

```
{{select placeholder="Priority" value="Medium"}}
  {{option value="Low"}}
  {{option value="Medium"}}
  {{option value="High"}}
{{/select}}
```

---

### textarea

```
{{textarea placeholder="..." rows="6"}}
```

Multi-line text field.

| Attribute | Required | Default | Description |
|-----------|----------|---------|-------------|
| `placeholder` | No | — | Hint text when empty |
| `value` | No | — | Current content. Use `\n` for line breaks |
| `rows` | No | `auto` | Height in lines. `auto` resizes to content |
| `cols` | No | `3` | Width in characters; `auto` = full width |
| `commit` | No | — | Boolean. Replaces the textarea with its content on entry |

```
{{textarea placeholder="Notes" cols="auto" rows="5"}}
```

---

## Container Directives

### note

```
{{note type="info" title="Heading" fold}}
  Content here
{{/note}}
```

Styled callout block with optional type, title, and action buttons.

| Attribute | Required | Default | Description |
|-----------|----------|---------|-------------|
| `type` | No | — | `info` \| `tip` \| `success` \| `warning` \| `error` — sets color scheme and icon |
| `title` | No | Type name | Heading. Defaults to the type name if `type` is set |
| `fold` | No | — | Boolean. Adds collapse/expand toggle |
| `commit` | No | — | Boolean. Adds button to replace note with its inner content |
| `copy` | No | — | Boolean. Adds button to copy content to clipboard |
| `remove` | No | — | Boolean. Adds button to delete the note |

```
{{note type="warning" fold}}
  Complete all required fields before proceeding.
{{/note}}

{{note type="tip" title="Pro tip" commit copy}}
  Use `Cmd/Ctrl + P` to commit all form values at once.
{{/note}}
```

---

### div

```
{{div shaded fold}}
  Content here
{{/div}}
```

Generic block container. Use for grouping, visual separation, or applying shared actions.

| Attribute | Required | Default | Description |
|-----------|----------|---------|-------------|
| `shaded` | No | — | Boolean. Applies a subtle gray background |
| `fold` | No | — | Boolean. Adds collapse/expand toggle |
| `commit` | No | — | Boolean. Adds button to unwrap content |
| `copy` | No | — | Boolean. Adds button to copy content to clipboard |
| `remove` | No | — | Boolean. Adds button to delete the div |

```
{{div shaded fold}}
  ## Meeting Notes
  {{textarea placeholder="Key discussion points..." rows="5"}}
{{/div}}
```

---

### span

```
{{span commit}}inline content{{/span}}
```

Inline container for wrapping text or inline elements. Useful for applying actions to a run of text.

| Attribute | Required | Default | Description |
|-----------|----------|---------|-------------|
| `commit` | No | — | Boolean. Adds button to unwrap content |
| `copy` | No | — | Boolean. Adds button to copy content to clipboard |
| `remove` | No | — | Boolean. Adds button to delete the span |

```
Name: {{span commit}}{{input placeholder="Enter name"}}{{/span}}
```

---

## Features

### Commit

Replaces a directive with its inner content or entered value, removing the directive markup.

- **Input / textarea:** Replaced by the entered value
- **Container (note, div, span):** Replaced by inner content only
- **Button:** Removed entirely
- Nested directives are committed from innermost outward

**Trigger:** Click the commit button on the directive, or press `Cmd/Ctrl + Alt + P` to commit all committable directives in the document.

### Copy

Copies a directive's content to the clipboard as HTML. Content is committed before copying, so form values are included.

### Fold

Collapses a directive to show only its header. Click again to expand. State is visual only — not stored in the document.

### Keyboard shortcuts

| Shortcut | Action |
|----------|--------|
| `Tab` | Next form element |
| `Shift + Tab` | Previous form element |
| `Cmd/Ctrl + Enter` | Exit current field |
| `Cmd/Ctrl + P` | Commit current field value |
| `Cmd/Ctrl + Backspace/Delete` | Remove directive |
| `Cmd/Ctrl + Alt + P` | Commit all directives in document |
| `Cmd/Ctrl + Alt + Q` | Toggle quick input mode |

> [!NOTE]
> In quick input mode, committing a value automatically moves focus to the next form field — useful for sequential data entry.
