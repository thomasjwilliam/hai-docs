# DirectiveObject

Represents a directive in the current document. Returned by [`hai.doc`](./hai-doc.md) query methods and [`doc.createDirective()`](./hai-doc.md#createdirective).

Attributes on the underlying directive are accessible directly as JavaScript properties.

---

## Properties

### tagName

```typescript
readonly tagName: string
```

The directive's tag name. Read-only.

```javascript
const dir = doc.getDirectiveById('box');
console.log(dir.tagName); // "div"
```

---

### innerText

```typescript
innerText: string | undefined
```

The text content between the opening and closing tags. `undefined` for self-closing directives (`input`, `button`, `textarea`, `option`).

> **Warning:** Setting `innerText` on a self-closing directive throws an error. Use attributes such as `placeholder` or `value` instead.

```javascript
const note = doc.getDirectiveById('my-note');
console.log(note.innerText);
note.innerText = 'Revised text';
```

---

### Dynamic attributes

Any attribute can be read, written, or deleted as a direct property on the object.

```javascript
const dir = doc.getDirectiveById('example');

// Read
console.log(dir.id);       // "example"
console.log(dir.class);    // "container main"

// Write
dir.class = 'highlight';
dir['data-order'] = '3';

// Boolean attributes — present/absent rather than string values
dir.required = true;   // adds the attribute
dir.required = false;  // removes the attribute
delete dir.class;      // also removes

// Reading a boolean attribute
console.log(dir.required);              // true (present) | undefined (absent)
console.log(dir.getAttribute('required')); // "" (present) | null (absent)
```

**Class manipulation note:** `class` is a space-separated string. To add or remove individual class names without clobbering others:

```javascript
const classes = dir.class ? dir.class.split(' ') : [];
if (!classes.includes('active')) {
  classes.push('active');
  dir.class = classes.join(' ');
}
```

---

## Methods

### getAttribute(name)

```typescript
getAttribute(name: string): string | null
```

| Parameter | Type | Description |
|-----------|------|-------------|
| `name` | `string` | Attribute name |

| Return value | Meaning |
|---|---|
| `string` | The attribute value |
| `""` (empty string) | Boolean attribute is present |
| `null` | Attribute is not present |

```javascript
dir.getAttribute('class');    // "container"
dir.getAttribute('disabled'); // "" if attribute is present
dir.getAttribute('missing');  // null
```

---

### setAttribute(name, val)

```typescript
setAttribute(name: string, val: string | boolean): void
```

Sets or removes an attribute. Passing `false`, `null`, or `undefined` removes the attribute.

| Parameter | Type | Description |
|-----------|------|-------------|
| `name` | `string` | Attribute name |
| `val` | `string \| boolean` | Value; `false`/`null`/`undefined` removes the attribute |

```javascript
dir.setAttribute('class', 'container');
dir.setAttribute('disabled', true);
dir.setAttribute('disabled', false); // removes disabled
```

---

### Scoped query methods

`DirectiveObject` exposes the same five search methods as [`hai.doc`](./hai-doc.md#query). They are identical in signature but search only within the subtree of this directive.

```typescript
getDirectiveById(id: string): DirectiveObject | undefined
getDirectiveByAttVal(att: string, val: string): DirectiveObject | undefined
getDirectivesByAttVal(att: string, val: string): DirectiveObject[]
getDirectivesByClass(cls: string): DirectiveObject[]
getDirectivesByTagName(tagName: string): DirectiveObject[]
```

See [`hai.doc` Query methods](./hai-doc.md#query) for full parameter descriptions.

```javascript
const form = doc.getDirectiveById('signup-form');

// Searches only inside form
const submit   = form.getDirectiveByAttVal('type', 'submit');
const inputs   = form.getDirectivesByTagName('input');
const required = form.getDirectivesByClass('required');
```

---

### insertAfter(dir)

```typescript
insertAfter(dir: DirectiveObject | string): void
```

Inserts `dir` immediately after this directive in the document.

| Parameter | Type | Description |
|-----------|------|-------------|
| `dir` | `DirectiveObject \| string` | Directive to insert |

```javascript
const anchor = doc.getDirectiveById('intro');
const separator = doc.createDirective('div');
separator.class = 'separator';
anchor.insertAfter(separator);
```

---

### insertBefore(dir)

```typescript
insertBefore(dir: DirectiveObject | string): void
```

Inserts `dir` immediately before this directive in the document.

| Parameter | Type | Description |
|-----------|------|-------------|
| `dir` | `DirectiveObject \| string` | Directive to insert |

```javascript
const footer = doc.getDirectiveById('footer');
footer.insertBefore('{{div class="pre-footer"}}Contact us{{/div}}');
```

---

### remove()

```typescript
remove(): void
```

Removes this directive from the document. The `DirectiveObject` instance is invalid after this call; do not use it further.

```javascript
const draft = doc.getDirectiveById('draft-note');
if (draft) {
  draft.remove();
  // draft is now invalid — don't access its properties
}
```

---

### replaceWith(dir)

```typescript
replaceWith(dir: DirectiveObject | string): void
```

Replaces this directive with another. The current instance is invalid after this call.

| Parameter | Type | Description |
|-----------|------|-------------|
| `dir` | `DirectiveObject \| string` | The replacement |

```javascript
const old = doc.getDirectiveById('v1-banner');
const updated = doc.createDirective('div');
updated.id = 'v2-banner';
updated.class = 'banner current';
updated.innerText = 'New banner content';
old.replaceWith(updated);
```

---

### toString()

```typescript
toString(): string
```

Returns the directive serialized to its `{{tagName ...}}content{{/tagName}}` string representation. Useful for debugging or building content strings.

```javascript
const dir = doc.getDirectiveById('summary');
console.log(dir.toString());
// {{div id="summary" class="box"}}Summary text{{/div}}
```

---

← [Back to index](./index.md)
