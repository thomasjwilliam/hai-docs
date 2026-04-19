# Macro Utility Library

The global `hai` object provides two utility namespaces available inside every macro: `hai.app` for notifications and `hai.doc` for reading and manipulating the current document's content and directives.

```javascript
const { app, doc } = hai;
```

---

## Quick Reference

### hai.app

| Method | Description |
|--------|-------------|
| [`notify(notification)`](#notify) | Display a notification to the user |

### hai.doc

| Method | Returns | Description |
|--------|---------|-------------|
| [`getContent()`](#getcontent) | `string` | Get the full document content |
| [`setContent(content)`](#setcontent) | `void` | Replace the entire document content |
| [`getTitle()`](#gettitle) | `string` | Get the document title |
| [`setTitle(title)`](#settitle) | `void` | Set the document title |
| [`createDirective(localName)`](#createdirective) | `DirectiveObject` | Create a new directive |
| [`getDirectiveById(id)`](#getdirectivebyid) | `DirectiveObject \| undefined` | Find a directive by its `id` attribute |
| [`getDirectiveByAttVal(att, val)`](#getdirectivebyattval) | `DirectiveObject \| undefined` | Find the first directive matching an attribute value |
| [`getDirectivesByAttVal(att, val)`](#getdirectivesbyattval) | `DirectiveObject[]` | Find all directives matching an attribute value |
| [`getDirectivesByClass(cls)`](#getdirectives byclass) | `DirectiveObject[]` | Find all directives with a given class |
| [`getDirectivesByTagName(tagName)`](#getdirectivesbytagname) | `DirectiveObject[]` | Find all directives with a given tag name |

### DirectiveObject

| Member | Type | Description |
|--------|------|-------------|
| [`tagName`](#tagname) | `string` (readonly) | The directive's tag name |
| [`innerText`](#innertext) | `string \| undefined` | Text content; `undefined` for self-closing directives |
| [dynamic attributes](#dynamic-attribute-access) | `string \| boolean \| undefined` | Get or set any attribute directly as a property |
| [`getAttribute(name)`](#getattribute) | `string \| null` | Read an attribute value |
| [`setAttribute(name, val)`](#setattribute) | `void` | Write or remove an attribute |
| [`getDirectiveById(id)`](#directiveobject-getdirectivebyid) | `DirectiveObject \| undefined` | Like `doc.getDirectiveById`, scoped to this directive |
| [`getDirectiveByAttVal(att, val)`](#directiveobject-getdirectivebyattval) | `DirectiveObject \| undefined` | Like `doc.getDirectiveByAttVal`, scoped to this directive |
| [`getDirectivesByAttVal(att, val)`](#directiveobject-getdirectivesbyattval) | `DirectiveObject[]` | Like `doc.getDirectivesByAttVal`, scoped to this directive |
| [`getDirectivesByClass(cls)`](#directiveobject-getdirectivesbyclass) | `DirectiveObject[]` | Like `doc.getDirectivesByClass`, scoped to this directive |
| [`getDirectivesByTagName(tagName)`](#directiveobject-getdirectivesbytagname) | `DirectiveObject[]` | Like `doc.getDirectivesByTagName`, scoped to this directive |
| [`insertAfter(dir)`](#insertafter) | `void` | Insert a directive immediately after this one |
| [`insertBefore(dir)`](#insertbefore) | `void` | Insert a directive immediately before this one |
| [`remove()`](#remove) | `void` | Remove this directive from the document |
| [`replaceWith(dir)`](#replacewith) | `void` | Replace this directive with another |
| [`toString()`](#tostring) | `string` | Serialize to directive syntax |

---

## hai.app

### notify

Displays a notification to the user.

```typescript
notify(notification: {
  message: string
  type: "info" | "success" | "warning" | "error"
  sticky?: boolean
  title?: string
}): void
```

**Parameters**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `message` | `string` | Yes | The notification body text |
| `type` | `"info" \| "success" \| "warning" \| "error"` | Yes | Visual style and severity |
| `sticky` | `boolean` | No | If `true`, stays until dismissed. Default: `false` |
| `title` | `string` | No | Optional heading above the message |

**Example**

```javascript
app.notify({ type: "success", title: "Done", message: "All items processed." });

app.notify({ type: "warning", message: "Review changes before saving.", sticky: true });
```

---

## hai.doc

### getContent

Returns the full document content as a string.

```typescript
getContent(): string
```

**Example**

```javascript
const content = doc.getContent();
console.log(content.length, 'characters');
```

---

### setContent

Replaces the entire document content.

```typescript
setContent(content: string): void
```

**Parameters**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `content` | `string` | Yes | The new document content |

**Example**

```javascript
doc.setContent('{{div id="root"}}Hello world{{/div}}');
```

---

### getTitle

Returns the document title.

```typescript
getTitle(): string
```

**Example**

```javascript
console.log(doc.getTitle());
```

---

### setTitle

Sets the document title.

```typescript
setTitle(title: string): void
```

**Parameters**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `title` | `string` | Yes | The new title |

**Example**

```javascript
doc.setTitle('Q4 Report – Draft');
```

---

### createDirective

Creates a new directive that can be inserted into the document. Self-closing tag names (`input`, `textarea`, `button`, `option`) automatically produce directives without closing tags.

```typescript
createDirective(localName: string): DirectiveObject
```

**Parameters**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `localName` | `string` | Yes | Tag name, e.g. `'div'`, `'note'`, `'input'` |

**Returns** — A new `DirectiveObject`. It is not yet part of the document; use [`insertAfter`](#insertafter), [`insertBefore`](#insertbefore), or [`replaceWith`](#replacewith) to place it.

**Example**

```javascript
const note = doc.createDirective('note');
note.id = 'reminder';
note.innerText = 'Check this before sending.';

const section = doc.getDirectiveById('intro');
section.insertAfter(note);
```

---

### getDirectiveById

Finds the directive with the given `id` attribute anywhere in the document.

```typescript
getDirectiveById(id: string): DirectiveObject | undefined
```

**Parameters**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `id` | `string` | Yes | Value of the `id` attribute to find |

**Returns** — The matching `DirectiveObject`, or `undefined` if not found.

**Example**

```javascript
const header = doc.getDirectiveById('page-header');
if (header) {
  header.class = 'active';
}
```

---

### getDirectiveByAttVal

Finds the first directive whose named attribute equals the given value.

```typescript
getDirectiveByAttVal(att: string, val: string): DirectiveObject | undefined
```

**Parameters**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `att` | `string` | Yes | Attribute name |
| `val` | `string` | Yes | Attribute value to match |

**Returns** — The first matching `DirectiveObject`, or `undefined`.

**Example**

```javascript
const primary = doc.getDirectiveByAttVal('role', 'primary');
```

---

### getDirectivesByAttVal

Finds all directives whose named attribute equals the given value.

```typescript
getDirectivesByAttVal(att: string, val: string): DirectiveObject[]
```

**Parameters**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `att` | `string` | Yes | Attribute name |
| `val` | `string` | Yes | Attribute value to match |

**Returns** — Array of matching `DirectiveObject` instances (may be empty).

**Example**

```javascript
const pending = doc.getDirectivesByAttVal('status', 'pending');
pending.forEach(item => { item.status = 'done'; });
```

---

### getDirectivesByClass

Finds all directives that include the given class name in their `class` attribute.

```typescript
getDirectivesByClass(cls: string): DirectiveObject[]
```

**Parameters**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `cls` | `string` | Yes | A single class name to match |

**Returns** — Array of matching `DirectiveObject` instances.

**Example**

```javascript
const highlighted = doc.getDirectivesByClass('highlight');
highlighted.forEach(el => {
  el.class = el.class.replace('highlight', '').trim();
});
```

---

### getDirectivesByTagName

Finds all directives with the given tag name.

```typescript
getDirectivesByTagName(tagName: string): DirectiveObject[]
```

**Parameters**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `tagName` | `string` | Yes | Tag name to match, e.g. `'note'`, `'div'` |

**Returns** — Array of matching `DirectiveObject` instances.

**Example**

```javascript
const notes = doc.getDirectivesByTagName('note');
console.log(`${notes.length} notes in document`);
```

---

## DirectiveObject

Represents a directive in the document. Returned by `hai.doc` query methods and `createDirective`. Attributes can be read and written directly as JavaScript properties.

### tagName

The tag name of the directive. Read-only.

**Type:** `string` (readonly)

```javascript
const dir = doc.getDirectiveById('box');
console.log(dir.tagName); // "div"
```

---

### innerText

The text content of the directive. `undefined` for self-closing directives (`input`, `button`, etc.). Setting `innerText` on a self-closing directive throws an error.

**Type:** `string | undefined`

```javascript
const note = doc.getDirectiveById('my-note');
console.log(note.innerText);
note.innerText = 'Updated content';
```

---

### Dynamic Attribute Access

Any attribute can be read, set, or deleted directly as a property.

```javascript
const dir = doc.getDirectiveById('example');

// Read
console.log(dir.id);           // "example"
console.log(dir.class);        // "container main"
console.log(dir.disabled);     // true | undefined

// Write
dir.class = 'new-class';
dir['data-value'] = '42';

// Boolean attributes
dir.disabled = true;           // adds disabled
dir.disabled = false;          // removes disabled

// Delete
delete dir.class;              // removes class attribute
```

---

### getAttribute

Returns the value of an attribute, or `null` if absent. Boolean attributes return an empty string `""`.

```typescript
getAttribute(name: string): string | null
```

**Parameters**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `name` | `string` | Yes | Attribute name |

```javascript
dir.getAttribute('class');     // "container"
dir.getAttribute('disabled');  // "" (boolean attribute present)
dir.getAttribute('missing');   // null
```

---

### setAttribute

Sets or removes an attribute. Pass `false`, `null`, or `undefined` to remove.

```typescript
setAttribute(name: string, val: string | boolean): void
```

**Parameters**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `name` | `string` | Yes | Attribute name |
| `val` | `string \| boolean` | Yes | Value to set; `false`/`null`/`undefined` removes the attribute |

```javascript
dir.setAttribute('class', 'container');
dir.setAttribute('disabled', true);
dir.setAttribute('disabled', false); // removes it
```

---

### DirectiveObject — Scoped Query Methods

`DirectiveObject` exposes the same five query methods as `hai.doc`, but they search only within the content of this directive rather than the entire document.

| Method | Equivalent `hai.doc` method |
|--------|-----------------------------|
| `getDirectiveById(id)` | [`doc.getDirectiveById`](#getdirectivebyid) |
| `getDirectiveByAttVal(att, val)` | [`doc.getDirectiveByAttVal`](#getdirectivebyattval) |
| `getDirectivesByAttVal(att, val)` | [`doc.getDirectivesByAttVal`](#getdirectivesbyattval) |
| `getDirectivesByClass(cls)` | [`doc.getDirectivesByClass`](#getdirectivesbyclass) |
| `getDirectivesByTagName(tagName)` | [`doc.getDirectivesByTagName`](#getdirectivesbytagname) |

Signatures and return types are identical. See the `hai.doc` entries above for full parameter details.

```javascript
const form = doc.getDirectiveById('contact-form');
const inputs = form.getDirectivesByTagName('input'); // only inside form
```

---

### insertAfter

Inserts a directive or raw directive string immediately after this directive in the document.

```typescript
insertAfter(dir: DirectiveObject | string): void
```

**Parameters**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `dir` | `DirectiveObject \| string` | Yes | What to insert |

```javascript
const anchor = doc.getDirectiveById('section-1');
const newNote = doc.createDirective('note');
newNote.innerText = 'See also section 2.';
anchor.insertAfter(newNote);
```

---

### insertBefore

Inserts a directive or raw directive string immediately before this directive in the document.

```typescript
insertBefore(dir: DirectiveObject | string): void
```

**Parameters**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `dir` | `DirectiveObject \| string` | Yes | What to insert |

```javascript
const anchor = doc.getDirectiveById('conclusion');
anchor.insertBefore('{{div class="separator"}}{{/div}}');
```

---

### remove

Removes this directive from the document. The `DirectiveObject` instance is invalid after this call.

```typescript
remove(): void
```

```javascript
const stale = doc.getDirectiveById('draft-note');
if (stale) stale.remove();
// Do not use stale after this point
```

---

### replaceWith

Replaces this directive with another directive or raw directive string.

```typescript
replaceWith(dir: DirectiveObject | string): void
```

**Parameters**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `dir` | `DirectiveObject \| string` | Yes | The replacement |

```javascript
const old = doc.getDirectiveById('v1-header');
const next = doc.createDirective('div');
next.id = 'v2-header';
next.innerText = 'Updated header';
old.replaceWith(next);
```

---

### toString

Returns the directive serialized to its `{{tagName ...}}...{{/tagName}}` string representation.

```typescript
toString(): string
```

```javascript
const dir = doc.getDirectiveById('my-div');
console.log(dir.toString());
// {{div id="my-div" class="container"}}Content{{/div}}
```

---

<details>
<summary>Pitfalls and best practices</summary>

**Self-closing directives have no `innerText`**
Tags like `input`, `textarea`, `button`, and `option` cannot hold text. Setting `innerText` on them throws. Use attributes (`placeholder`, `value`) instead.

**Boolean attributes**
- Read: `dir.disabled` returns `true` if present, `undefined` if absent.
- `dir.getAttribute('disabled')` returns `""` if present, `null` if absent.
- Write `false` or call `delete dir.disabled` to remove.

**Invalidated references**
After calling `remove()` or being replaced by `replaceWith()`, the `DirectiveObject` instance must not be used again.

**Class manipulation**
`dir.class` is a space-separated string — not a list. Use `split`/`filter`/`join` to add or remove individual classes rather than overwriting the whole value.

**Always check for `undefined`**
`getDirectiveById` and `getDirectiveByAttVal` return `undefined` when not found. Guard before accessing properties.

</details>
