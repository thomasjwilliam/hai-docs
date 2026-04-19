# Markdown Directives

Markdown directives are custom syntax elements that embed interactive UI components — form inputs, buttons, and containers — directly in documents. They are rendered in place and can be read and manipulated by macros via [`hai.doc`](../../macro-utilities/version-c-multipage/hai-doc.md).

**Syntax:**
```
{{tagName attribute="value" booleanAttribute}}
{{tagName attribute="value"}}content{{/tagName}}
```

## Pages

| Page | Contents |
|------|----------|
| [Global Attributes](./global-attributes.md) | `id`, `class`, `aria-label`, built-in class behaviors |
| [Form Directives](./form-directives.md) | `button`, `checkbox-group`, `datalist`, `input`, `option`, `radio-group`, `select`, `textarea` |
| [Container Directives](./container-directives.md) | `note`, `div`, `span` |
| [Features](./features.md) | Commit, Copy, Fold, Keyboard shortcuts |

## Directive index

### Form directives

| Directive | Self-closing | Description |
|-----------|-------------|-------------|
| [`button`](./form-directives.md#button) | Yes | Triggers a macro or snippet on click |
| [`checkbox-group`](./form-directives.md#checkbox-group) | No | Group of toggleable checkboxes |
| [`datalist`](./form-directives.md#datalist) | No | Searchable dropdown with free-text fallback |
| [`input`](./form-directives.md#input) | Yes | Single-line text, date, or datetime field |
| [`option`](./form-directives.md#option) | Yes | An item within a group directive |
| [`radio-group`](./form-directives.md#radio-group) | No | Single-selection button group |
| [`select`](./form-directives.md#select) | No | Fixed-choice dropdown |
| [`textarea`](./form-directives.md#textarea) | Yes | Multi-line text field |

### Container directives

| Directive | Description |
|-----------|-------------|
| [`note`](./container-directives.md#note) | Styled callout block with optional type, title, and actions |
| [`div`](./container-directives.md#div) | Generic block container with optional visual styling and actions |
| [`span`](./container-directives.md#span) | Inline container for text or inline elements |

## Quick start

```
# Customer Form

Name: {{input id="name" placeholder="Full name"}}
Email: {{input placeholder="email@example.com"}}

Contact preference:
{{radio-group value="email"}}
  {{option value="email" label="Email"}}
  {{option value="phone" label="Phone"}}
{{/radio-group}}

Notes:
{{textarea placeholder="Additional notes..." rows="4" cols="auto"}}

{{button onclick="$SubmitForm" text="Submit" color="primary"}}
```

## Saving directives as snippets

Instead of typing directives manually, save common patterns as snippets. A snippet named `DatePicker` containing `{{input type="date"}}` can be inserted anywhere with `/DatePicker` and referenced from macros as `$DatePicker`.
