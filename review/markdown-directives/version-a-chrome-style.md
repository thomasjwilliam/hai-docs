# Markdown Directives

Markdown directives are custom syntax elements that add interactive UI components — form inputs, buttons, and containers — directly into documents. They follow a consistent double-brace syntax and integrate with macros via the `hai.doc` library.

**Syntax:**
```
{{tagName attribute="value" booleanAttribute}}
{{tagName attribute="value"}}content{{/tagName}}
```

---

## Quick Reference

### Form directives

| Directive | Self-closing | Description |
|-----------|-------------|-------------|
| [`button`](#button) | Yes | Triggers a macro or snippet on click |
| [`checkbox-group`](#checkbox-group) | No | Group of toggleable checkboxes |
| [`datalist`](#datalist) | No | Searchable dropdown with free-text fallback |
| [`input`](#input) | Yes | Single-line text, date, or datetime field |
| [`option`](#option) | Yes | An item within `checkbox-group`, `datalist`, `radio-group`, or `select` |
| [`radio-group`](#radio-group) | No | Single-selection button group |
| [`select`](#select) | No | Fixed-choice dropdown |
| [`textarea`](#textarea) | Yes | Multi-line text field |

### Container directives

| Directive | Description |
|-----------|-------------|
| [`note`](#note) | Styled callout block with optional type, title, and action buttons |
| [`div`](#div) | Generic block container with optional visual and action features |
| [`span`](#span) | Inline container for text or inline elements |

### Global attributes

| Attribute | Applies to | Description |
|-----------|-----------|-------------|
| [`id`](#id) | All | Unique identifier; used with `hai.doc` query methods |
| [`class`](#class) | All | Space-separated CSS class names; also controls built-in behaviors |
| [`aria-label`](#aria-label) | All | Accessibility label for screen readers |

### Built-in class names

| Class | Effect |
|-------|--------|
| `commit` | Adds a button to replace the element with its inner content |
| `copy` | Adds a button to copy content to clipboard (as HTML) |
| `fold` | Adds a collapse/expand toggle |
| `remove` | Adds a button to delete the element |
| `primary`, `secondary`, `tertiary` | Presentation styles |
| `info`, `tip`, `success`, `warning`, `error` | Semantic color styles |

---

## Global Attributes

### id

A unique identifier for the directive within the document. Required when targeting the element with macros.

```
{{input id="customer-name"}}
{{div id="summary-section"}}content{{/div}}
```

---

### class

Space-separated CSS class names. In addition to custom styling, several built-in class names enable interactive behaviors (see [Built-in class names](#built-in-class-names) above).

```
{{div class="fold"}}Collapsible section{{/div}}
{{div class="commit copy"}}Content with two actions{{/div}}
```

---

### aria-label

Accessibility label for screen readers. Defaults to visible text (e.g., button text) when not set.

```
{{button aria-label="Submit the contact form" onclick="$SubmitForm" text="Submit"}}
```

---

## Form Directives

### button

Triggers a snippet or macro when clicked.

**Syntax:** `{{button ...attributes}}`

**Attributes**

| Attribute | Required | Default | Description |
|-----------|----------|---------|-------------|
| `onclick` | Yes | — | Snippet or macro trigger. Format: `$Name` or `$Name[ordinal]` |
| `text` | Yes | — | Label displayed on the button |
| `color` | No | `primary` | One of `primary` \| `secondary` \| `accent` \| `info` \| `success` \| `warning` \| `error` |
| `aria-label` | No | Button text | Accessibility label |
| `id`, `class` | No | — | Global attributes |

**Example**

```
{{button onclick="$SaveDraft" text="Save"}}
{{button onclick="$DeleteItem" text="Delete" color="error"}}
{{button onclick="$ProcessOrder[2]" text="Process" color="success"}}
```

---

### checkbox-group

Renders a group of checkboxes. Each option is a nested [`option`](#option) directive.

**Syntax:**
```
{{checkbox-group ...attributes}}
  {{option value="..."}}
  {{option value="..." label="..."}}
{{/checkbox-group}}
```

**Attributes**

| Attribute | Required | Default | Description |
|-----------|----------|---------|-------------|
| `value` | No | — | Comma-separated list of checked values |
| `column` | No | — | Boolean. Stacks checkboxes vertically |
| `aria-label`, `id`, `class` | No | — | Global attributes |

**Example**

```
{{checkbox-group value="email,sms" column}}
  {{option value="email" label="Email"}}
  {{option value="sms" label="SMS"}}
  {{option value="phone" label="Phone"}}
{{/checkbox-group}}
```

---

### datalist

A searchable dropdown that also accepts free-text entry.

**Syntax:**
```
{{datalist ...attributes}}
  {{option value="..."}}
{{/datalist}}
```

**Attributes**

| Attribute | Required | Default | Description |
|-----------|----------|---------|-------------|
| `placeholder` | No | — | Hint text when nothing is selected |
| `value` | No | — | Currently selected or entered value |
| `aria-label`, `id`, `class` | No | — | Global attributes |

**Example**

```
{{datalist placeholder="Select your role"}}
  {{option value="Developer"}}
  {{option value="Designer"}}
  {{option value="Manager"}}
{{/datalist}}
```

---

### input

Single-line text, date, or datetime-local field.

**Syntax:** `{{input ...attributes}}`

**Attributes**

| Attribute | Required | Default | Description |
|-----------|----------|---------|-------------|
| `type` | No | `text` | `text` \| `date` \| `datetime-local` |
| `placeholder` | No | — | Hint text when empty |
| `value` | No | — | Current value |
| `cols` | No | `12` | Width in characters. Use `auto` for 100% width |
| `commit` | No | — | Boolean. Replaces the input with its value on entry |
| `aria-label`, `id`, `class` | No | — | Global attributes |

**Example**

```
{{input type="text" placeholder="Full name" id="name"}}
{{input type="date"}}
{{input type="text" cols="auto" placeholder="Full-width field"}}
{{input commit placeholder="Quick entry"}}
```

---

### option

A single option used inside `checkbox-group`, `datalist`, `radio-group`, or `select`.

**Syntax:** `{{option ...attributes}}`

**Attributes**

| Attribute | Required | Default | Description |
|-----------|----------|---------|-------------|
| `value` | Yes | — | The option's value |
| `label` | No | Same as `value` | Display text shown to the user |
| `aria-label`, `id`, `class` | No | — | Global attributes |

**Example**

```
{{option value="xl" label="Extra Large"}}
```

---

### radio-group

Single-selection button group. Each option is a nested [`option`](#option) directive.

**Syntax:**
```
{{radio-group ...attributes}}
  {{option value="..."}}
{{/radio-group}}
```

**Attributes**

| Attribute | Required | Default | Description |
|-----------|----------|---------|-------------|
| `value` | No | — | Value of the selected option |
| `column` | No | — | Boolean. Stacks radio buttons vertically |
| `aria-label`, `id`, `class` | No | — | Global attributes |

**Example**

```
{{radio-group value="medium" column}}
  {{option value="low" label="Low"}}
  {{option value="medium" label="Medium"}}
  {{option value="high" label="High"}}
{{/radio-group}}
```

---

### select

Fixed-choice dropdown. Each option is a nested [`option`](#option) directive.

**Syntax:**
```
{{select ...attributes}}
  {{option value="..."}}
{{/select}}
```

**Attributes**

| Attribute | Required | Default | Description |
|-----------|----------|---------|-------------|
| `placeholder` | No | — | Hint text when nothing is selected |
| `value` | No | — | Value of the selected option |
| `aria-label`, `id`, `class` | No | — | Global attributes |

**Example**

```
{{select placeholder="Priority" value="Medium"}}
  {{option value="Low"}}
  {{option value="Medium"}}
  {{option value="High"}}
{{/select}}
```

---

### textarea

Multi-line text input.

**Syntax:** `{{textarea ...attributes}}`

**Attributes**

| Attribute | Required | Default | Description |
|-----------|----------|---------|-------------|
| `placeholder` | No | — | Hint text when empty |
| `value` | No | — | Current content (`\n` for line breaks) |
| `rows` | No | `auto` | Height in lines. `auto` resizes to content |
| `cols` | No | `3` | Width in characters. `auto` for full width |
| `commit` | No | — | Boolean. Replaces the textarea with its content on entry |
| `aria-label`, `id`, `class` | No | — | Global attributes |

**Example**

```
{{textarea placeholder="Describe the issue..." rows="6" cols="auto"}}
```

---

## Container Directives

### note

A styled callout block with optional type-specific colors and action buttons.

**Syntax:**
```
{{note ...attributes}}
  content
{{/note}}
```

**Attributes**

| Attribute | Required | Default | Description |
|-----------|----------|---------|-------------|
| `type` | No | — | `info` \| `tip` \| `success` \| `warning` \| `error` — sets color and icon |
| `title` | No | Type name | Heading text. Defaults to the type name when `type` is set |
| `fold` | No | — | Boolean. Adds collapse/expand toggle |
| `commit` | No | — | Boolean. Adds button to unwrap content |
| `copy` | No | — | Boolean. Adds button to copy content to clipboard |
| `remove` | No | — | Boolean. Adds button to delete the note |
| `aria-label`, `id`, `class` | No | — | Global attributes |

**Example**

```
{{note type="warning" title="Before you proceed" fold}}
  Check that all required fields are filled in before submitting.
{{/note}}
```

---

### div

Generic block container with optional visual emphasis and action buttons.

**Syntax:**
```
{{div ...attributes}}
  content
{{/div}}
```

**Attributes**

| Attribute | Required | Default | Description |
|-----------|----------|---------|-------------|
| `shaded` | No | — | Boolean. Applies a subtle gray background |
| `fold` | No | — | Boolean. Adds collapse/expand toggle |
| `commit` | No | — | Boolean. Adds button to unwrap content |
| `copy` | No | — | Boolean. Adds button to copy content to clipboard |
| `remove` | No | — | Boolean. Adds button to delete the div |
| `aria-label`, `id`, `class` | No | — | Global attributes |

**Example**

```
{{div shaded fold}}
  ## Action Items
  {{textarea placeholder="List action items..." rows="4"}}
{{/div}}
```

---

### span

Inline container for wrapping text or inline elements with optional action buttons.

**Syntax:** `{{span ...attributes}}inline content{{/span}}`

**Attributes**

| Attribute | Required | Default | Description |
|-----------|----------|---------|-------------|
| `commit` | No | — | Boolean. Adds button to unwrap content |
| `copy` | No | — | Boolean. Adds button to copy content to clipboard |
| `remove` | No | — | Boolean. Adds button to delete the span |
| `aria-label`, `id`, `class` | No | — | Global attributes |

**Example**

```
Customer {{span commit}}{{input placeholder="Name"}}{{/span}} placed an order.
```

---

## Keyboard Shortcuts

| Action | Shortcut |
|--------|----------|
| Commit all directives in document | `Cmd/Ctrl + Alt + P` |
| Toggle quick input mode | `Cmd/Ctrl + Alt + Q` |
| Exit input field | `Cmd/Ctrl + Enter` |
| Commit current field value | `Cmd/Ctrl + P` |
| Remove directive | `Cmd/Ctrl + Backspace/Delete` |
| Next form element | `Tab` |
| Previous form element | `Shift + Tab` |

---

<details>
<summary>Tips and best practices</summary>

- **Save frequent patterns as snippets.** A snippet named `DatePicker` containing `{{input type="date"}}` can be inserted anywhere with `/DatePicker`.
- **Use meaningful `id` values.** Descriptive IDs make it easier to target directives with `hai.doc.getDirectiveById()`.
- **Group related fields.** Wrap related inputs in a `div` or `note` for logical organization and shared actions.
- **Use `commit` for finalization.** Templates that should produce plain text output should use `commit` on containers.
- **Add `fold` to long sections.** Collapsible sections keep complex documents manageable.
- **Enable quick input mode** (`Cmd/Ctrl + Alt + Q`) for rapid sequential form filling.
- **Choose the right input type.** Use `type="date"` or `type="datetime-local"` instead of text fields for temporal data.

</details>
