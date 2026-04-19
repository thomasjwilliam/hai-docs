# Macro Utility Library

The `hai` global is available in every macro. It exposes two objects:

| Object | Purpose |
|--------|---------|
| `hai.app` | User-facing interactions (notifications) |
| `hai.doc` | Read and manipulate the current document |

```javascript
const { app, doc } = hai;
```

Directive query methods (`getDirectiveBy*`) return [`DirectiveObject`](#directiveobject) instances — representations of template elements with the syntax `{{tagName attr="value"}}content{{/tagName}}`.

---

## hai.app

### `notify(notification)`

```typescript
app.notify(notification: {
  message: string
  type: "info" | "success" | "warning" | "error"
  sticky?: boolean
  title?: string
}): void
```

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `message` | `string` | — | Notification body |
| `type` | `"info" \| "success" \| "warning" \| "error"` | — | Visual style |
| `sticky` | `boolean` | `false` | Stay until dismissed |
| `title` | `string` | — | Optional heading |

```javascript
app.notify({ type: "error", title: "Failed", message: "Could not save." });
app.notify({ type: "info", message: "Processing…", sticky: true });
```

---

## hai.doc

### `getContent()`

```typescript
doc.getContent(): string
```

Returns the full document content as a string.

```javascript
const raw = doc.getContent();
```

---

### `setContent(content)`

```typescript
doc.setContent(content: string): void
```

Replaces all document content.

```javascript
doc.setContent('{{note}}Hello{{/note}}');
```

---

### `getTitle()`

```typescript
doc.getTitle(): string
```

Returns the current document title.

---

### `setTitle(title)`

```typescript
doc.setTitle(title: string): void
```

Sets the document title.

```javascript
doc.setTitle('Meeting Notes – 2025-06-01');
```

---

### `createDirective(localName)`

```typescript
doc.createDirective(localName: string): DirectiveObject
```

Creates a new `DirectiveObject`. The directive is not in the document until you call `insertAfter`, `insertBefore`, or `replaceWith`.

Self-closing tag names — `input`, `textarea`, `button`, `option` — produce directives without a closing tag.

```javascript
const input = doc.createDirective('input');
input.type = 'text';
input.placeholder = 'Name';

const section = doc.getDirectiveById('form-section');
section.insertAfter(input);
```

---

### `getDirectiveById(id)`

```typescript
doc.getDirectiveById(id: string): DirectiveObject | undefined
```

Finds the directive with the matching `id` attribute. Returns `undefined` if not found.

```javascript
const el = doc.getDirectiveById('summary');
if (el) el.class = 'active';
```

---

### `getDirectiveByAttVal(att, val)`

```typescript
doc.getDirectiveByAttVal(att: string, val: string): DirectiveObject | undefined
```

Finds the **first** directive where attribute `att` equals `val`. Returns `undefined` if not found.

```javascript
const featured = doc.getDirectiveByAttVal('data-role', 'featured');
```

---

### `getDirectivesByAttVal(att, val)`

```typescript
doc.getDirectivesByAttVal(att: string, val: string): DirectiveObject[]
```

Finds **all** directives where attribute `att` equals `val`. Returns an empty array if none match.

```javascript
const pending = doc.getDirectivesByAttVal('status', 'pending');
pending.forEach(item => { item.status = 'done'; });
```

---

### `getDirectivesByClass(cls)`

```typescript
doc.getDirectivesByClass(cls: string): DirectiveObject[]
```

Finds all directives whose `class` attribute contains `cls` as a whole class name.

```javascript
const alerts = doc.getDirectivesByClass('alert');
```

---

### `getDirectivesByTagName(tagName)`

```typescript
doc.getDirectivesByTagName(tagName: string): DirectiveObject[]
```

Finds all directives with the given tag name.

```javascript
const notes = doc.getDirectivesByTagName('note');
```

---

## DirectiveObject

Returned by all `hai.doc` query methods and `createDirective`. Represents a directive in the document tree.

### Properties

#### `tagName`

```typescript
readonly tagName: string
```

The directive's tag name. Read-only.

---

#### `innerText`

```typescript
innerText: string | undefined
```

Text content of the directive. `undefined` for self-closing directives.

> [!WARNING]
> Setting `innerText` on a self-closing directive (`input`, `button`, etc.) throws an error.

```javascript
const note = doc.getDirectiveById('my-note');
note.innerText = 'Revised text';
```

---

#### Dynamic attributes

Any attribute can be read or written as a direct property. Attributes not present in the directive return `undefined`.

```javascript
dir.class = 'highlight';
dir['data-id'] = '42';

// Boolean attributes
dir.required = true;   // adds the attribute
dir.required = false;  // removes it

delete dir.class;      // also removes
```

> [!NOTE]
> Reading a boolean attribute via property access returns `true` when present and `undefined` when absent. Use `getAttribute` if you need the `""` / `null` distinction.

---

### `getAttribute(name)`

```typescript
getAttribute(name: string): string | null
```

| Return value | Meaning |
|---|---|
| `string` | The attribute value |
| `""` (empty string) | Boolean attribute is present |
| `null` | Attribute is absent |

```javascript
dir.getAttribute('class');     // "container active"
dir.getAttribute('disabled');  // ""
dir.getAttribute('missing');   // null
```

---

### `setAttribute(name, val)`

```typescript
setAttribute(name: string, val: string | boolean): void
```

Sets an attribute. Pass `false`, `null`, or `undefined` as `val` to remove the attribute.

```javascript
dir.setAttribute('class', 'container');
dir.setAttribute('disabled', true);
dir.setAttribute('disabled', false); // removes it
```

---

### Scoped query methods

`DirectiveObject` exposes the same five search methods as `hai.doc`. They behave identically but search only within the subtree of this directive.

```typescript
getDirectiveById(id: string): DirectiveObject | undefined
getDirectiveByAttVal(att: string, val: string): DirectiveObject | undefined
getDirectivesByAttVal(att: string, val: string): DirectiveObject[]
getDirectivesByClass(cls: string): DirectiveObject[]
getDirectivesByTagName(tagName: string): DirectiveObject[]
```

```javascript
const form = doc.getDirectiveById('signup-form');
const emailInput = form.getDirectiveByAttVal('type', 'email');
const allInputs = form.getDirectivesByTagName('input');
```

---

### `insertAfter(dir)`

```typescript
insertAfter(dir: DirectiveObject | string): void
```

Inserts `dir` immediately after this directive in the document.

```javascript
const anchor = doc.getDirectiveById('intro');
const divider = doc.createDirective('div');
divider.class = 'separator';
anchor.insertAfter(divider);
```

---

### `insertBefore(dir)`

```typescript
insertBefore(dir: DirectiveObject | string): void
```

Inserts `dir` immediately before this directive in the document.

```javascript
doc.getDirectiveById('footer').insertBefore('{{div class="pre-footer"}}{{/div}}');
```

---

### `remove()`

```typescript
remove(): void
```

Removes this directive from the document.

> [!WARNING]
> The `DirectiveObject` instance is invalid after `remove()` returns. Do not read or write properties on it afterward.

```javascript
const draft = doc.getDirectiveById('draft-note');
if (draft) draft.remove();
```

---

### `replaceWith(dir)`

```typescript
replaceWith(dir: DirectiveObject | string): void
```

Replaces this directive with `dir`. The current instance is invalid afterward.

```javascript
const legacy = doc.getDirectiveById('old-header');
const updated = doc.createDirective('div');
updated.id = 'new-header';
updated.innerText = 'Welcome';
legacy.replaceWith(updated);
```

---

### `toString()`

```typescript
toString(): string
```

Returns the directive serialized to `{{tagName ...}}content{{/tagName}}` form.

```javascript
console.log(doc.getDirectiveById('summary').toString());
// {{div id="summary" class="box"}}Summary text{{/div}}
```
