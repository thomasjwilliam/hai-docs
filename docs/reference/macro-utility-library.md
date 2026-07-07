# Macro Utility Library

The global `hai` library gives macros access to application context, document
content, and notifications. It exposes two namespaces — `app` and `doc` — and
a `DirectiveObject` type returned by most `doc` methods.

**Syntax:**

```javascript
const {app, doc} = hai
```

---

## Quick Reference

### hai.app

| Member              | Kind   | Description                         |
| ------------------- | ------ | ----------------------------------- |
| [`notify`](#notify) | Method | Displays a notification to the user |

### hai.doc

| Member                                                        | Kind     | Description                                         |
| ------------------------------------------------------------- | -------- | --------------------------------------------------- |
| [`docTitle`](#doctitle)                                       | Property | The document title                                  |
| [`docContent`](#doccontent)                                   | Property | The document content                                |
| [`getTitle`](#gettitle)                                       | Method   | Gets the document title                             |
| [`setTitle`](#settitle)                                       | Method   | Sets the document title                             |
| [`getContent`](#getcontent)                                   | Method   | Gets the document content                           |
| [`setContent`](#setcontent)                                   | Method   | Replaces the document content                       |
| [`appendContent`](#appendcontent)                             | Method   | Appends content to the end of the document          |
| [`prependContent`](#prependcontent)                           | Method   | Inserts content at the start of the document        |
| [`copyToClipboard`](#copytoclipboard)                         | Method   | Copies text to the clipboard                        |
| [`createDirective`](#createdirective)                         | Method   | Creates a new directive                             |
| [`getDirectiveById (doc)`](#getdirectivebyid-doc)             | Method   | Finds a directive in the document by `id`           |
| [`getDirectiveByAttVal (doc)`](#getdirectivebyattval-doc)     | Method   | Finds the first directive with a matching attribute |
| [`getDirectivesByAttVal (doc)`](#getdirectivesbyattval-doc)   | Method   | Finds all directives with a matching attribute      |
| [`getDirectivesByClass (doc)`](#getdirectivesbyclass-doc)     | Method   | Finds all directives with a class                   |
| [`getDirectivesByTagName (doc)`](#getdirectivesbytagname-doc) | Method   | Finds all directives with a tag name                |

### DirectiveObject

| Member                                                                    | Kind                 | Description                                                                        |
| ------------------------------------------------------------------------- | -------------------- | ---------------------------------------------------------------------------------- |
| [`tagName`](#tagname)                                                     | Property (read-only) | The directive's tag name                                                           |
| [`innerMD`](#innermd)                                                     | Property             | Raw inner Markdown source of the directive                                         |
| [`innerText`](#innertext-deprecated)                                      | Property             | Deprecated alias for `innerMD`                                                     |
| [`value`](#value)                                                         | Property             | On `textarea`, alias for `innerMD`; on other tags, alias for the `value` attribute |
| [`getAttribute`](#getattribute)                                           | Method               | Reads an attribute value                                                           |
| [`setAttribute`](#setattribute)                                           | Method               | Sets or removes an attribute                                                       |
| [`getDirectiveById (directive)`](#getdirectivebyid-directive)             | Method               | Finds a nested directive by `id`                                                   |
| [`getDirectiveByAttVal (directive)`](#getdirectivebyattval-directive)     | Method               | Finds the first nested directive with a matching attribute                         |
| [`getDirectivesByAttVal (directive)`](#getdirectivesbyattval-directive)   | Method               | Finds all nested directives with a matching attribute                              |
| [`getDirectivesByClass (directive)`](#getdirectivesbyclass-directive)     | Method               | Finds all nested directives with a class                                           |
| [`getDirectivesByTagName (directive)`](#getdirectivesbytagname-directive) | Method               | Finds all nested directives with a tag name                                        |
| [`insertBefore`](#insertbefore)                                           | Method               | Inserts content immediately before this directive                                  |
| [`insertAfter`](#insertafter)                                             | Method               | Inserts content immediately after this directive                                   |
| [`replaceWith`](#replacewith)                                             | Method               | Replaces this directive                                                            |
| [`remove`](#remove)                                                       | Method               | Removes this directive from the document                                           |
| [`toString`](#tostring)                                                   | Method               | Serializes the directive back to `{{tagName ...}}` text                            |

---

## Directive Syntax

Directives are the template elements that make up a document:

```
{{tagName attr="value" booleanAttr}}content{{/tagName}}
```

Self-closing directives (`input`, `button`, `option`) have no closing tag. All
other tag names — including `textarea`, `note`, `div`, and any custom name —
are block directives with an opening and closing tag:

```
{{input type="text" id="name"}}
```

**Examples:**

```
{{div id="container" class="main"}}
  Some content here
{{/div}}

{{note remove}}Important note{{/note}}

{{input type="text" placeholder="Enter name"}}
```

See [Markdown Directives](./markdown-directives) for the full directive
reference.

---

## hai.app

User-facing notifications.

### notify

Displays a notification to the user.

**Syntax:**

```typescript
notify(notification: {
  message: string
  type: "info" | "success" | "warning" | "error"
  sticky?: boolean
  title?: string
}): void
```

**Parameters**

| Parameter              | Type      | Required | Default | Description                                             |
| ---------------------- | --------- | -------- | ------- | ------------------------------------------------------- |
| `notification.message` | `string`  | Yes      | —       | The main notification message                           |
| `notification.type`    | `string`  | Yes      | —       | One of `info` \| `success` \| `warning` \| `error`      |
| `notification.sticky`  | `boolean` | No       | `false` | Keeps the notification visible until manually dismissed |
| `notification.title`   | `string`  | No       | —       | Optional title shown above the message                  |

**Example:**

```javascript
// Simple info notification
app.notify({
  type: "info",
  message: "Processing started...",
})

// Success notification with title
app.notify({
  type: "success",
  title: "Complete!",
  message: "All tasks have been processed",
})

// Sticky warning that requires dismissal
app.notify({
  type: "warning",
  message: "Please review the changes before saving",
  sticky: true,
})
```

---

## hai.doc

Utilities for reading and manipulating document content, title, and
directives. Methods that locate directives return `DirectiveObject`
instances — see [DirectiveObject](#directiveobject).

### docTitle

The document title. Writable directly, or via [`setTitle`](#settitle) — both
update the same underlying value.

**Type:** `string`

**Example:**

```javascript
console.log("Current title:", doc.docTitle)
doc.docTitle = "My Updated Document"
```

---

### docContent

The document content. Writable directly, or via [`setContent`](#setcontent) —
both update the same underlying value.

**Type:** `string`

**Example:**

```javascript
console.log("Document has", doc.docContent.length, "characters")
doc.docContent = "{{div}}New document content{{/div}}"
```

---

### getTitle

Gets the current document title.

**Syntax:**

```typescript
getTitle(): string
```

**Returns:** The document title.

**Example:**

```javascript
const title = doc.getTitle()
console.log("Document title:", title)
```

---

### setTitle

Sets the document title.

**Syntax:**

```typescript
setTitle(title: string | DirectiveObject): void
```

**Parameters**

| Parameter | Type                        | Required | Default | Description            |
| --------- | --------------------------- | -------- | ------- | ---------------------- |
| `title`   | `string \| DirectiveObject` | Yes      | —       | The new document title |

**Example:**

```javascript
doc.setTitle("My Updated Document")
```

---

### getContent

Gets the current document content.

**Syntax:**

```typescript
getContent(): string
```

**Returns:** The document content.

**Example:**

```javascript
const content = doc.getContent()
console.log("Document has", content.length, "characters")
```

---

### setContent

Replaces the document content.

**Syntax:**

```typescript
setContent(content: string | DirectiveObject): void
```

**Parameters**

| Parameter | Type                        | Required | Default | Description              |
| --------- | --------------------------- | -------- | ------- | ------------------------ |
| `content` | `string \| DirectiveObject` | Yes      | —       | The new document content |

**Example:**

```javascript
doc.setContent("{{div}}New document content{{/div}}")
```

---

### appendContent

Appends content to the end of the document.

**Syntax:**

```typescript
appendContent(content: string | DirectiveObject): void
```

**Parameters**

| Parameter | Type                        | Required | Default | Description           |
| --------- | --------------------------- | -------- | ------- | --------------------- |
| `content` | `string \| DirectiveObject` | Yes      | —       | The content to append |

**Example:**

```javascript
const summary = doc.createDirective("note")
summary.innerText = "Generated summary"
doc.appendContent(summary)
```

---

### prependContent

Inserts content at the start of the document.

**Syntax:**

```typescript
prependContent(content: string | DirectiveObject): void
```

**Parameters**

| Parameter | Type                        | Required | Default | Description            |
| --------- | --------------------------- | -------- | ------- | ---------------------- |
| `content` | `string \| DirectiveObject` | Yes      | —       | The content to prepend |

**Example:**

```javascript
doc.prependContent('{{note type="info"}}Generated by macro{{/note}}')
```

---

### copyToClipboard

Copies text to the clipboard.

**Syntax:**

```typescript
copyToClipboard(options?: {
  text?: string | DirectiveObject
  format?: "plain" | "html"
}): void
```

**Parameters**

| Parameter        | Type                        | Required | Default          | Description                                                         |
| ---------------- | --------------------------- | -------- | ---------------- | ------------------------------------------------------------------- |
| `options.text`   | `string \| DirectiveObject` | No       | Document content | Text to copy. Defaults to the current document content when omitted |
| `options.format` | `"plain" \| "html"`         | No       | `"plain"`        | Clipboard format                                                    |

**Example:**

```javascript
// Copy the current document content as plain text
doc.copyToClipboard()

// Copy specific HTML content
doc.copyToClipboard({text: "<b>Hello</b>", format: "html"})
```

---

### createDirective

Creates a new directive that can be inserted into the document.

**Syntax:**

```typescript
createDirective(localName: string): DirectiveObject
```

**Parameters**

| Parameter   | Type     | Required | Default | Description                                          |
| ----------- | -------- | -------- | ------- | ---------------------------------------------------- |
| `localName` | `string` | Yes      | —       | The tag name for the directive (e.g. `div`, `input`) |

**Returns:** A new `DirectiveObject`. Self-closing tags (`input`, `button`,
`option`) are created without a closing tag. All other tag names — including
`textarea` — are created as a block directive with an opening and closing
tag.

**Example:**

```javascript
const newDiv = doc.createDirective("div")
newDiv.id = "my-div"
newDiv.class = "container"
newDiv.innerText = "Hello World"

const existingDiv = doc.getDirectiveById("parent")
existingDiv.insertAfter(newDiv)
```

---

### getDirectiveById (doc)

Finds a directive in the document by its `id` attribute.

**Syntax:**

```typescript
getDirectiveById(id: string): DirectiveObject | undefined
```

**Parameters**

| Parameter | Type     | Required | Default | Description          |
| --------- | -------- | -------- | ------- | -------------------- |
| `id`      | `string` | Yes      | —       | The id to search for |

**Returns:** The matching `DirectiveObject`, or `undefined` if not found.

**Example:**

```javascript
const container = doc.getDirectiveById("main-container")
if (container) {
  container.class = "active"
}
```

---

### getDirectiveByAttVal (doc)

Finds the first directive in the document with a specific attribute value.

**Syntax:**

```typescript
getDirectiveByAttVal(att: string, val: string): DirectiveObject | undefined
```

**Parameters**

| Parameter | Type     | Required | Default | Description                      |
| --------- | -------- | -------- | ------- | -------------------------------- |
| `att`     | `string` | Yes      | —       | The attribute name to search for |
| `val`     | `string` | Yes      | —       | The attribute value to match     |

**Returns:** The matching `DirectiveObject`, or `undefined` if not found.

**Example:**

```javascript
const important = doc.getDirectiveByAttVal("data-type", "important")
if (important) {
  console.log("Found:", important.tagName)
}
```

---

### getDirectivesByAttVal (doc)

Finds all directives in the document with a specific attribute value.

**Syntax:**

```typescript
getDirectivesByAttVal(att: string, val: string): DirectiveObject[]
```

**Parameters**

| Parameter | Type     | Required | Default | Description                      |
| --------- | -------- | -------- | ------- | -------------------------------- |
| `att`     | `string` | Yes      | —       | The attribute name to search for |
| `val`     | `string` | Yes      | —       | The attribute value to match     |

**Returns:** Array of matching `DirectiveObject` instances.

**Example:**

```javascript
const pending = doc.getDirectivesByAttVal("status", "pending")
pending.forEach((item) => {
  item.status = "completed"
})
```

---

### getDirectivesByClass (doc)

Finds all directives in the document with a specific CSS class name.

**Syntax:**

```typescript
getDirectivesByClass(cls: string): DirectiveObject[]
```

**Parameters**

| Parameter | Type     | Required | Default | Description                                                         |
| --------- | -------- | -------- | ------- | ------------------------------------------------------------------- |
| `cls`     | `string` | Yes      | —       | The class name to search for (matched within space-separated lists) |

**Returns:** Array of matching `DirectiveObject` instances.

**Example:**

```javascript
const highlighted = doc.getDirectivesByClass("highlight")
highlighted.forEach((item) => {
  item.class = item.class.replace("highlight", "").trim()
})
```

---

### getDirectivesByTagName (doc)

Finds all directives in the document with a specific tag name.

**Syntax:**

```typescript
getDirectivesByTagName(tagName: string): DirectiveObject[]
```

**Parameters**

| Parameter | Type     | Required | Default | Description                              |
| --------- | -------- | -------- | ------- | ---------------------------------------- |
| `tagName` | `string` | Yes      | —       | The tag name to search for (e.g. `note`) |

**Returns:** Array of matching `DirectiveObject` instances.

**Example:**

```javascript
const notes = doc.getDirectivesByTagName("note")
notes.forEach((note) => {
  note.remove = true
})
```

---

## DirectiveObject

Represents a directive in the document. Attributes can be read and written
directly as object properties — see [Dynamic attribute
access](#dynamic-attribute-access).

### tagName

The tag name of the directive.

**Type:** `string` (read-only)

**Example:**

```javascript
const dir = doc.getDirectiveById("my-div")
console.log(dir.tagName) // "div"
```

---

### innerMD

The raw inner Markdown source inside the directive — the `innerHTML` analogue
for Markdown. Returns `""` when empty, including for self-closing directives
(never `undefined`). Setting it on a self-closing directive throws.

**Type:** `string | DirectiveObject`

**Example:**

```javascript
const note = doc.getDirectiveById("my-note")
console.log(note.innerMD)
note.innerMD = "Updated note content"

const input = doc.getDirectivesByTagName("input")[0]
console.log(input.innerMD) // ""  (self-closing, DOM parity)
```

---

### innerText (deprecated)

Deprecated alias for `innerMD`, kept so existing scripts keep working. Returns
and sets the identical value. Prefer `innerMD` in new scripts.

---

### value

DOM-parity property whose meaning depends on the tag:

- On `textarea`, it's an alias for [`innerMD`](#innermd) — leading/trailing
  single newlines from the block markdown syntax are trimmed on read.
  Setting it writes the inner content, same as `innerMD`.
- On any other tag (e.g. `input`), it's a plain alias for the `value`
  attribute — equivalent to `getAttribute("value")` /
  `setAttribute("value", ...)`.

**Type:** `string | undefined`

**Example:**

```javascript
const textarea = doc.getDirectivesByTagName("textarea")[0]
textarea.value = "Multi-line\ncontent"
console.log(textarea.value) // "Multi-line\ncontent"

const input = doc.getDirectivesByTagName("input")[0]
input.value = "default text" // same as input.setAttribute("value", "default text")
```

---

### Dynamic attribute access

Any directive attribute can be read or written directly as a property,
without going through `getAttribute`/`setAttribute`.

**Example:**

```javascript
const dir = doc.getDirectiveById("example")

// Reading attributes
console.log(dir.id) // "example"
console.log(dir.class) // "container main"
console.log(dir.disabled) // true (for boolean attributes)

// Setting attributes
dir.class = "new-class"
dir["data-value"] = "123"

// Boolean attributes
dir.disabled = true // adds the attribute
dir.disabled = false // removes it

// Removing attributes
delete dir.class
```

---

### getAttribute

Gets the value of a specified attribute.

**Syntax:**

```typescript
getAttribute(name: string): string | null
```

**Parameters**

| Parameter | Type     | Required | Default | Description        |
| --------- | -------- | -------- | ------- | ------------------ |
| `name`    | `string` | Yes      | —       | The attribute name |

**Returns:** The attribute value, an empty string for boolean attributes, or
`null` if the attribute is not set.

**Example:**

```javascript
const dir = doc.getDirectiveById("my-div")

dir.getAttribute("id") // "my-div"
dir.getAttribute("disabled") // "" (boolean attribute is present)
dir.getAttribute("nonexistent") // null
```

---

### setAttribute

Sets or removes an attribute.

**Syntax:**

```typescript
setAttribute(name: string, val: string | boolean): void
```

**Parameters**

| Parameter | Type                | Required | Default | Description                                                                                           |
| --------- | ------------------- | -------- | ------- | ----------------------------------------------------------------------------------------------------- |
| `name`    | `string`            | Yes      | —       | The attribute name                                                                                    |
| `val`     | `string \| boolean` | Yes      | —       | The value to set. Use `true` for boolean attributes; `false`/`null`/`undefined` removes the attribute |

**Example:**

```javascript
const dir = doc.getDirectiveById("my-div")

dir.setAttribute("class", "container")
dir.setAttribute("disabled", true)
dir.setAttribute("disabled", false) // removes it
```

---

### getDirectiveById (directive)

Searches this directive's content for a nested directive by `id`.

**Syntax:**

```typescript
getDirectiveById(id: string): DirectiveObject | undefined
```

**Parameters**

| Parameter | Type     | Required | Default | Description          |
| --------- | -------- | -------- | ------- | -------------------- |
| `id`      | `string` | Yes      | —       | The id to search for |

**Returns:** The matching `DirectiveObject`, or `undefined` if not found.

**Example:**

```javascript
const container = doc.getDirectiveById("container")
const nested = container.getDirectiveById("nested-item")
```

---

### getDirectiveByAttVal (directive)

Searches this directive's content for the first nested directive with a
specific attribute value.

**Syntax:**

```typescript
getDirectiveByAttVal(att: string, val: string): DirectiveObject | undefined
```

**Parameters**

| Parameter | Type     | Required | Default | Description                      |
| --------- | -------- | -------- | ------- | -------------------------------- |
| `att`     | `string` | Yes      | —       | The attribute name to search for |
| `val`     | `string` | Yes      | —       | The attribute value to match     |

**Returns:** The matching `DirectiveObject`, or `undefined` if not found.

**Example:**

```javascript
const form = doc.getDirectiveById("my-form")
const submitBtn = form.getDirectiveByAttVal("type", "submit")
```

---

### getDirectivesByAttVal (directive)

Searches this directive's content for all nested directives with a specific
attribute value.

**Syntax:**

```typescript
getDirectivesByAttVal(att: string, val: string): DirectiveObject[]
```

**Parameters**

| Parameter | Type     | Required | Default | Description                      |
| --------- | -------- | -------- | ------- | -------------------------------- |
| `att`     | `string` | Yes      | —       | The attribute name to search for |
| `val`     | `string` | Yes      | —       | The attribute value to match     |

**Returns:** Array of matching `DirectiveObject` instances.

**Example:**

```javascript
const container = doc.getDirectiveById("task-list")
const pending = container.getDirectivesByAttVal("status", "pending")
```

---

### getDirectivesByClass (directive)

Searches this directive's content for all nested directives with a specific
class name.

**Syntax:**

```typescript
getDirectivesByClass(cls: string): DirectiveObject[]
```

**Parameters**

| Parameter | Type     | Required | Default | Description                  |
| --------- | -------- | -------- | ------- | ---------------------------- |
| `cls`     | `string` | Yes      | —       | The class name to search for |

**Returns:** Array of matching `DirectiveObject` instances.

**Example:**

```javascript
const section = doc.getDirectiveById("main-section")
const alerts = section.getDirectivesByClass("alert")
```

---

### getDirectivesByTagName (directive)

Searches this directive's content for all nested directives with a specific
tag name.

**Syntax:**

```typescript
getDirectivesByTagName(tagName: string): DirectiveObject[]
```

**Parameters**

| Parameter | Type     | Required | Default | Description                |
| --------- | -------- | -------- | ------- | -------------------------- |
| `tagName` | `string` | Yes      | —       | The tag name to search for |

**Returns:** Array of matching `DirectiveObject` instances.

**Example:**

```javascript
const form = doc.getDirectiveById("my-form")
const inputs = form.getDirectivesByTagName("input")
inputs.forEach((input) => {
  input.required = true
})
```

---

### insertBefore

Inserts a directive or string immediately before this directive.

**Syntax:**

```typescript
insertBefore(dir: DirectiveObject | string): void
```

**Parameters**

| Parameter | Type                        | Required | Default | Description             |
| --------- | --------------------------- | -------- | ------- | ----------------------- |
| `dir`     | `DirectiveObject \| string` | Yes      | —       | The directive to insert |

**Example:**

```javascript
const div = doc.getDirectiveById("my-div")

const newNote = doc.createDirective("note")
newNote.innerText = "Added before"
div.insertBefore(newNote)

div.insertBefore("{{div}}Another div{{/div}}")
```

---

### insertAfter

Inserts a directive or string immediately after this directive.

**Syntax:**

```typescript
insertAfter(dir: DirectiveObject | string): void
```

**Parameters**

| Parameter | Type                        | Required | Default | Description             |
| --------- | --------------------------- | -------- | ------- | ----------------------- |
| `dir`     | `DirectiveObject \| string` | Yes      | —       | The directive to insert |

**Example:**

```javascript
const div = doc.getDirectiveById("my-div")

const newNote = doc.createDirective("note")
newNote.innerText = "Added after"
div.insertAfter(newNote)

div.insertAfter("{{div}}Another div{{/div}}")
```

---

### replaceWith

Replaces this directive with another directive or string.

**Syntax:**

```typescript
replaceWith(dir: DirectiveObject | string): void
```

**Parameters**

| Parameter | Type                        | Required | Default | Description                   |
| --------- | --------------------------- | -------- | ------- | ----------------------------- |
| `dir`     | `DirectiveObject \| string` | Yes      | —       | The directive to replace with |

**Example:**

```javascript
const oldDiv = doc.getDirectiveById("old-div")

const newDiv = doc.createDirective("div")
newDiv.id = "new-div"
newDiv.innerText = "Replaced content"
oldDiv.replaceWith(newDiv)
```

---

### remove

Removes this directive from the document.

**Syntax:**

```typescript
remove(): void
```

**Warning:** The `DirectiveObject` instance becomes invalid after removal —
do not use it afterwards.

**Example:**

```javascript
const oldDiv = doc.getDirectiveById("obsolete")
if (oldDiv) {
  oldDiv.remove()
  // don't use oldDiv after this point
}
```

---

### toString

Returns the string representation of the directive.

**Syntax:**

```typescript
toString(): string
```

**Returns:** The directive as a string, e.g. `{{tagName attr="value"}}...{{/tagName}}`.

**Example:**

```javascript
const div = doc.getDirectiveById("my-div")
console.log(div.toString())
// {{div id="my-div" class="container"}}Content{{/div}}
```

---

<details>
<summary>Tips and best practices</summary>

- **Check for existence before manipulating.** `getDirectiveById` and similar
  lookups return `undefined` when nothing matches — guard before accessing
  properties.
- **Use the right search method.** `getDirectiveById` for unique elements,
  `getDirectivesByTagName` for all elements of a type, `getDirectivesByClass`
  for elements sharing a class, `getDirectivesByAttVal` for custom attributes.
- **Prefer property access for simple changes** (`dir.class = "active"`) over
  `setAttribute` — both work, but property access reads better for everyday use.
- **Don't overwrite space-separated classes.** Split, modify, and rejoin
  instead of assigning a single new value: `dir.class = (dir.class ?? "").split(" ").concat("new").join(" ")`.
- **Self-closing directives have no `innerText`/`innerMD`.** Setting either on
  `input`, `button`, or `option` throws — use their attributes instead.
  `textarea` is a block directive and _can_ hold text — use its
  [`value`](#value) property.
- **Don't reuse a `DirectiveObject` after calling `remove()`** — the instance
  becomes invalid.
- **Use `createDirective()` for dynamic content**, then `insertBefore`/`insertAfter`/`replaceWith` to place it.

</details>
