# Container Directives

Container directives wrap content to provide structure, visual styling, and interactive actions. All container directives support the [global attributes](./global-attributes.md) (`id`, `class`, `aria-label`).

---

## note

```
{{note type="info" title="Heading" fold}}
  Content here
{{/note}}
```

A styled callout block with optional type-specific color, a title, and action buttons. The most visually distinct container directive — use it for alerts, tips, warnings, and summaries.

### Attributes

| Attribute | Required | Default | Description |
|-----------|----------|---------|-------------|
| `type` | No | — | Sets color scheme and icon: `info` (blue) \| `tip` (green) \| `success` (green) \| `warning` (orange) \| `error` (red) |
| `title` | No | Type name | Heading text. Defaults to the type name when `type` is set |
| `fold` | No | — | Boolean. Adds a collapse/expand toggle |
| `commit` | No | — | Boolean. Adds a button to replace the note with its inner content |
| `copy` | No | — | Boolean. Adds a button to copy content to clipboard (as HTML) |
| `remove` | No | — | Boolean. Adds a button to delete the note |

### Examples

**Unstyled note:**
```
{{note}}
  This is a plain note without type styling.
{{/note}}
```

**Warning with collapse:**
```
{{note type="warning" fold}}
  Complete all required fields before proceeding.
{{/note}}
```

**Success with custom title:**
```
{{note type="success" title="Submission received"}}
  Your request has been processed. Reference: #12345
{{/note}}
```

**Note with all actions:**
```
{{note type="tip" title="Did you know?" fold commit copy remove}}
  Press `Cmd/Ctrl + Alt + P` to commit all directives at once.
{{/note}}
```

**Containing form elements:**
```
{{note type="info" title="Support Ticket" fold}}
  Customer: {{input placeholder="Name"}}
  Issue type: {{select placeholder="Select..."}}
    {{option value="bug" label="Bug report"}}
    {{option value="question" label="Question"}}
  {{/select}}
  {{textarea placeholder="Describe the issue..." rows="4"}}
  {{button onclick="$CreateTicket" text="Submit" color="success"}}
{{/note}}
```

---

## div

```
{{div shaded fold}}
  Content here
{{/div}}
```

Generic block container. Use `div` when you need structural grouping or shared actions without the visual emphasis of a `note`.

### Attributes

| Attribute | Required | Default | Description |
|-----------|----------|---------|-------------|
| `shaded` | No | — | Boolean. Applies a subtle gray background for visual separation |
| `fold` | No | — | Boolean. Adds a collapse/expand toggle |
| `commit` | No | — | Boolean. Adds a button to replace the div with its inner content |
| `copy` | No | — | Boolean. Adds a button to copy content to clipboard (as HTML) |
| `remove` | No | — | Boolean. Adds a button to delete the div |

### Examples

**Simple group:**
```
{{div}}
  Name: {{input placeholder="Full name"}}
  Email: {{input placeholder="email@example.com"}}
{{/div}}
```

**Shaded section with collapse:**
```
{{div shaded fold}}
  ## Action Items
  {{textarea placeholder="List action items and owners..." rows="4"}}
{{/div}}
```

**Nested sections:**
```
{{div class="form-section" shaded}}
  Name: {{input type="text"}}
  Email: {{input type="text"}}

  {{div class="subsection"}}
    Date of Birth: {{input type="date"}}
  {{/div}}
{{/div}}
```

---

## span

```
{{span commit}}inline content{{/span}}
```

Inline container for wrapping a run of text or inline elements. Use `span` when you need to apply actions to part of a sentence rather than a block.

### Attributes

| Attribute | Required | Default | Description |
|-----------|----------|---------|-------------|
| `commit` | No | — | Boolean. Adds a button to replace the span with its inner content |
| `copy` | No | — | Boolean. Adds a button to copy content to clipboard (as HTML) |
| `remove` | No | — | Boolean. Adds a button to delete the span |

### Examples

**Inline input with commit:**
```
The order was placed by {{span commit}}{{input placeholder="Customer name"}}{{/span}}.
```

**Highlight text for removal:**
```
This sentence contains {{span remove}}a removable clause{{/span}} that can be deleted.
```

---

← [Back to index](./index.md)
