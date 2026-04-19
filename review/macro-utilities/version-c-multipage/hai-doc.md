# hai.doc

Document utilities for reading content, updating the title, and finding or creating directives. Access via `hai.doc` or destructuring:

```javascript
const { doc } = hai;
```

Query methods return [`DirectiveObject`](./directive-object.md) instances.

---

## Content

### getContent()

```typescript
doc.getContent(): string
```

Returns the full document content as a string.

```javascript
const raw = doc.getContent();
console.log(raw.length, 'characters');
```

---

### setContent(content)

```typescript
doc.setContent(content: string): void
```

Replaces the entire document content with the given string.

| Parameter | Type | Description |
|-----------|------|-------------|
| `content` | `string` | The new content |

```javascript
doc.setContent('{{note}}Document reset.{{/note}}');
```

---

### getTitle()

```typescript
doc.getTitle(): string
```

Returns the current document title.

```javascript
const title = doc.getTitle();
```

---

### setTitle(title)

```typescript
doc.setTitle(title: string): void
```

Sets the document title.

| Parameter | Type | Description |
|-----------|------|-------------|
| `title` | `string` | The new title |

```javascript
doc.setTitle('Q4 Review – Final');
```

---

## Factory

### createDirective(localName)

```typescript
doc.createDirective(localName: string): DirectiveObject
```

Creates a new [`DirectiveObject`](./directive-object.md). The directive is not part of the document until placed with [`insertAfter`](./directive-object.md#insertafter), [`insertBefore`](./directive-object.md#insertbefore), or [`replaceWith`](./directive-object.md#replacewith).

| Parameter | Type | Description |
|-----------|------|-------------|
| `localName` | `string` | Tag name, e.g. `'div'`, `'note'`, `'input'` |

Self-closing tag names (`input`, `textarea`, `button`, `option`) automatically produce directives without a closing tag.

```javascript
const note = doc.createDirective('note');
note.id = 'reminder';
note.innerText = 'Check this before sending.';

const section = doc.getDirectiveById('intro');
section.insertAfter(note);
```

---

## Query

All query methods return `undefined` or an empty array when nothing is found — they never throw.

### getDirectiveById(id)

```typescript
doc.getDirectiveById(id: string): DirectiveObject | undefined
```

Returns the directive with the given `id` attribute, or `undefined` if not found.

| Parameter | Type | Description |
|-----------|------|-------------|
| `id` | `string` | Value of the `id` attribute |

```javascript
const header = doc.getDirectiveById('page-header');
if (header) {
  header.class = 'visible';
}
```

---

### getDirectiveByAttVal(att, val)

```typescript
doc.getDirectiveByAttVal(att: string, val: string): DirectiveObject | undefined
```

Returns the **first** directive where attribute `att` equals `val`, or `undefined`.

| Parameter | Type | Description |
|-----------|------|-------------|
| `att` | `string` | Attribute name |
| `val` | `string` | Attribute value to match |

```javascript
const primary = doc.getDirectiveByAttVal('data-role', 'primary');
```

---

### getDirectivesByAttVal(att, val)

```typescript
doc.getDirectivesByAttVal(att: string, val: string): DirectiveObject[]
```

Returns **all** directives where attribute `att` equals `val`. Returns an empty array if none match.

| Parameter | Type | Description |
|-----------|------|-------------|
| `att` | `string` | Attribute name |
| `val` | `string` | Attribute value to match |

```javascript
const pending = doc.getDirectivesByAttVal('status', 'pending');
pending.forEach(item => { item.status = 'done'; });
```

---

### getDirectivesByClass(cls)

```typescript
doc.getDirectivesByClass(cls: string): DirectiveObject[]
```

Returns all directives whose `class` attribute contains `cls` as a complete class name (space-separated matching).

| Parameter | Type | Description |
|-----------|------|-------------|
| `cls` | `string` | A single class name |

```javascript
const alerts = doc.getDirectivesByClass('alert');
alerts.forEach(el => {
  el.class = el.class.replace('alert', '').trim();
});
```

---

### getDirectivesByTagName(tagName)

```typescript
doc.getDirectivesByTagName(tagName: string): DirectiveObject[]
```

Returns all directives with the given tag name.

| Parameter | Type | Description |
|-----------|------|-------------|
| `tagName` | `string` | Tag name, e.g. `'note'`, `'div'`, `'input'` |

```javascript
const notes = doc.getDirectivesByTagName('note');
console.log(`${notes.length} notes found`);
```

---

← [Back to index](./index.md)
