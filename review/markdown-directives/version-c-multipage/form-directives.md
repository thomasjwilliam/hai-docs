# Form Directives

Form directives provide interactive input elements for collecting data. All form directives support the [global attributes](./global-attributes.md) (`id`, `class`, `aria-label`).

---

## button

```
{{button onclick="$MacroName" text="Label"}}
```

Triggers a snippet or macro when clicked.

### Attributes

| Attribute | Required | Default | Description |
|-----------|----------|---------|-------------|
| `onclick` | Yes | — | Snippet or macro trigger, prefixed with `$`. Use `$Name[n]` to select the nth match when multiple snippets share the same name |
| `text` | Yes | — | Label displayed on the button |
| `color` | No | `primary` | `primary` \| `secondary` \| `accent` \| `info` \| `success` \| `warning` \| `error` |
| `aria-label` | No | Button text | Overrides the implicit accessibility label |

### Examples

```
{{button onclick="$SaveDraft" text="Save"}}
{{button onclick="$DeleteItem" text="Delete" color="error"}}
{{button onclick="$ProcessOrder[2]" text="Process" color="success"}}
```

**With form fields:**
```
First Name: {{input id="first-name" placeholder="John"}}
Last Name: {{input id="last-name" placeholder="Doe"}}
{{button onclick="$CreateProfile" text="Create Profile"}}
```

---

## checkbox-group

```
{{checkbox-group value="a,b"}}
  {{option value="a" label="Option A"}}
  {{option value="b" label="Option B"}}
  {{option value="c" label="Option C"}}
{{/checkbox-group}}
```

Renders a group of individually toggleable checkboxes. Define items with nested [`option`](#option) directives.

### Attributes

| Attribute | Required | Default | Description |
|-----------|----------|---------|-------------|
| `value` | No | — | Comma-separated list of checked values. Commas within values are automatically escaped |
| `column` | No | — | Boolean. Displays checkboxes stacked vertically instead of horizontally |

### Example

```
{{checkbox-group value="email,sms" column}}
  {{option value="email" label="Email Notifications"}}
  {{option value="sms" label="SMS Notifications"}}
  {{option value="phone" label="Phone Calls"}}
{{/checkbox-group}}
```

---

## datalist

```
{{datalist placeholder="Select or type..."}}
  {{option value="..."}}
{{/datalist}}
```

A searchable dropdown that also accepts free-text input not present in the list. Define options with nested [`option`](#option) directives.

### Attributes

| Attribute | Required | Default | Description |
|-----------|----------|---------|-------------|
| `placeholder` | No | — | Hint text when nothing is entered or selected |
| `value` | No | — | Currently selected or entered value |

### Example

```
{{datalist placeholder="Choose a country" value="France"}}
  {{option value="France"}}
  {{option value="Germany"}}
  {{option value="Italy"}}
  {{option value="Spain"}}
{{/datalist}}
```

---

## input

```
{{input type="text" placeholder="..."}}
```

Single-line field for text, dates, or date-times.

### Attributes

| Attribute | Required | Default | Description |
|-----------|----------|---------|-------------|
| `type` | No | `text` | `text` \| `date` \| `datetime-local` |
| `placeholder` | No | — | Hint text when the field is empty |
| `value` | No | — | Current field value |
| `cols` | No | `12` | Visible width in characters. `auto` = 100% container width |
| `commit` | No | — | Boolean. Immediately replaces the input with its entered value |

### Examples

```
{{input placeholder="Full name" id="customer-name"}}
```

```
{{input type="date"}}
```

```
{{input type="datetime-local" value="2025-06-01T09:00"}}
```

```
{{input type="text" cols="auto" placeholder="Full-width text input"}}
```

```
{{input commit placeholder="Type and press Enter to commit"}}
```

---

## option

```
{{option value="xl" label="Extra Large"}}
```

A single selectable item used inside [`checkbox-group`](#checkbox-group), [`datalist`](#datalist), [`radio-group`](#radio-group), or [`select`](#select). When `label` is omitted, the `value` is used as the display text.

### Attributes

| Attribute | Required | Default | Description |
|-----------|----------|---------|-------------|
| `value` | Yes | — | The option's stored value |
| `label` | No | Same as `value` | The text displayed to the user |

### Example

```
{{option value="md" label="Medium"}}
```

---

## radio-group

```
{{radio-group value="selected-value"}}
  {{option value="..."}}
{{/radio-group}}
```

Single-selection button group. Define items with nested [`option`](#option) directives.

### Attributes

| Attribute | Required | Default | Description |
|-----------|----------|---------|-------------|
| `value` | No | — | Value of the currently selected option |
| `column` | No | — | Boolean. Stacks options vertically |

### Example

```
{{radio-group value="medium" column}}
  {{option value="low" label="Low Priority"}}
  {{option value="medium" label="Medium Priority"}}
  {{option value="high" label="High Priority"}}
{{/radio-group}}
```

---

## select

```
{{select placeholder="Choose..."}}
  {{option value="..."}}
{{/select}}
```

Fixed-choice dropdown. Define options with nested [`option`](#option) directives. Unlike [`datalist`](#datalist), only listed options can be selected.

### Attributes

| Attribute | Required | Default | Description |
|-----------|----------|---------|-------------|
| `placeholder` | No | — | Hint text when nothing is selected |
| `value` | No | — | Currently selected value |

### Example

```
{{select placeholder="Select size" value="M"}}
  {{option value="S" label="Small"}}
  {{option value="M" label="Medium"}}
  {{option value="L" label="Large"}}
  {{option value="XL" label="Extra Large"}}
{{/select}}
```

---

## textarea

```
{{textarea placeholder="..." rows="5"}}
```

Multi-line text input. Auto-resizes to its content by default.

### Attributes

| Attribute | Required | Default | Description |
|-----------|----------|---------|-------------|
| `placeholder` | No | — | Hint text when the field is empty |
| `value` | No | — | Current content. Use `\n` for line breaks |
| `rows` | No | `auto` | Height in lines. `auto` = resizes to content |
| `cols` | No | `3` | Width in characters. `auto` = full container width |
| `commit` | No | — | Boolean. Immediately replaces the textarea with its content |

### Examples

```
{{textarea placeholder="Describe the issue..." cols="auto" rows="6"}}
```

```
{{textarea placeholder="Quick note" commit}}
```

---

← [Back to index](./index.md)
