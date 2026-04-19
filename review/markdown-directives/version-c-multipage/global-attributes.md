# Global Attributes

These attributes are available on all directives.

---

## id

```
{{input id="customer-name"}}
{{div id="summary-section"}}content{{/div}}
```

A unique identifier for the directive within the document. Used with [`hai.doc.getDirectiveById()`](../../macro-utilities/version-c-multipage/hai-doc.md#getdirectivebyid) in macros to target specific elements.

---

## class

```
{{div class="fold remove"}}content{{/div}}
```

Space-separated CSS class names. Some names enable built-in interactive behaviors (see below).

### Built-in behavior classes

| Class | Description |
|-------|-------------|
| `commit` | Adds a button that replaces the element with its inner content (unwraps it) |
| `copy` | Adds a button that copies the element's content to the clipboard as HTML |
| `fold` | Adds a collapse/expand toggle |
| `remove` | Adds a button that deletes the element and its content |

Multiple behavior classes can be combined:

```
{{div class="fold commit remove"}}
  This section can be folded, committed, or removed.
{{/div}}
```

### Built-in presentation classes

Apply semantic color styling:

| Class | Color |
|-------|-------|
| `primary` | Brand primary |
| `secondary` | Brand secondary |
| `tertiary` | Brand tertiary |
| `info` | Blue |
| `tip` | Green |
| `success` | Green |
| `warning` | Orange |
| `error` | Red |

---

## aria-label

```
{{button aria-label="Submit the contact form" onclick="$Submit" text="Submit"}}
```

Accessibility label for screen readers and assistive technologies. Overrides the implicit label derived from visible text (e.g., button text). Use when the visible label is ambiguous without surrounding context.

---

← [Back to index](./index.md)
