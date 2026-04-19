# Macro Utility Library

The `hai` global is available in every macro. It provides two utility objects for interacting with the application and the current document.

```javascript
const { app, doc } = hai;
```

## Objects

| Object | Description | Reference |
|--------|-------------|-----------|
| [`hai.app`](./hai-app.md) | Display notifications to the user | [hai.app →](./hai-app.md) |
| [`hai.doc`](./hai-doc.md) | Read and manipulate document content and directives | [hai.doc →](./hai-doc.md) |
| [`DirectiveObject`](./directive-object.md) | Represents a directive; returned by `hai.doc` query methods | [DirectiveObject →](./directive-object.md) |

## What is a directive?

Directives are template elements embedded in document content using double-brace syntax:

```
{{tagName attr="value" booleanAttr}}content{{/tagName}}
```

Self-closing directives (such as `input`, `button`, `textarea`) omit the closing tag:

```
{{input type="text" placeholder="Enter your name"}}
```

The `hai.doc` methods return and operate on [`DirectiveObject`](./directive-object.md) instances that represent these elements.

## Quick start

```javascript
const { app, doc } = hai;

// Read and update document content
const title = doc.getTitle();
doc.setTitle(title + ' (revised)');

// Find a directive and modify it
const banner = doc.getDirectiveById('alert-banner');
if (banner) {
  banner.class = 'alert-banner visible';
  banner.innerText = 'This document has been updated.';
}

// Create and insert a new directive
const note = doc.createDirective('note');
note.innerText = 'Please review before sending.';
banner.insertAfter(note);

// Notify the user
app.notify({ type: "success", message: "Document updated." });
```

## Method index

### hai.app

| Method | Description |
|--------|-------------|
| [`notify(notification)`](./hai-app.md#notify) | Display a notification to the user |

### hai.doc

| Method | Returns | Description |
|--------|---------|-------------|
| [`getContent()`](./hai-doc.md#getcontent) | `string` | Full document content |
| [`setContent(content)`](./hai-doc.md#setcontent) | `void` | Replace document content |
| [`getTitle()`](./hai-doc.md#gettitle) | `string` | Document title |
| [`setTitle(title)`](./hai-doc.md#settitle) | `void` | Set document title |
| [`createDirective(localName)`](./hai-doc.md#createdirective) | `DirectiveObject` | Create a new directive |
| [`getDirectiveById(id)`](./hai-doc.md#getdirectivebyid) | `DirectiveObject \| undefined` | Find directive by `id` |
| [`getDirectiveByAttVal(att, val)`](./hai-doc.md#getdirectivebyattval) | `DirectiveObject \| undefined` | Find first directive by attribute value |
| [`getDirectivesByAttVal(att, val)`](./hai-doc.md#getdirectivesbyattval) | `DirectiveObject[]` | Find all directives by attribute value |
| [`getDirectivesByClass(cls)`](./hai-doc.md#getdirectivesbyclass) | `DirectiveObject[]` | Find directives by class name |
| [`getDirectivesByTagName(tagName)`](./hai-doc.md#getdirectivesbytagname) | `DirectiveObject[]` | Find directives by tag name |

### DirectiveObject

| Member | Type | Description |
|--------|------|-------------|
| [`tagName`](./directive-object.md#tagname) | `string` (readonly) | The directive's tag name |
| [`innerText`](./directive-object.md#innertext) | `string \| undefined` | Text content |
| [*attribute*](./directive-object.md#dynamic-attributes) | `string \| boolean \| undefined` | Any attribute as a direct property |
| [`getAttribute(name)`](./directive-object.md#getattribute) | `string \| null` | Read an attribute |
| [`setAttribute(name, val)`](./directive-object.md#setattribute) | `void` | Write or remove an attribute |
| [`getDirectiveById`](./directive-object.md#scoped-query-methods) | `DirectiveObject \| undefined` | Scoped search by id |
| [`getDirectiveByAttVal`](./directive-object.md#scoped-query-methods) | `DirectiveObject \| undefined` | Scoped search by attribute value |
| [`getDirectivesByAttVal`](./directive-object.md#scoped-query-methods) | `DirectiveObject[]` | Scoped search by attribute value |
| [`getDirectivesByClass`](./directive-object.md#scoped-query-methods) | `DirectiveObject[]` | Scoped search by class |
| [`getDirectivesByTagName`](./directive-object.md#scoped-query-methods) | `DirectiveObject[]` | Scoped search by tag name |
| [`insertAfter(dir)`](./directive-object.md#insertafter) | `void` | Insert after this directive |
| [`insertBefore(dir)`](./directive-object.md#insertbefore) | `void` | Insert before this directive |
| [`remove()`](./directive-object.md#remove) | `void` | Remove from document |
| [`replaceWith(dir)`](./directive-object.md#replacewith) | `void` | Replace with another directive |
| [`toString()`](./directive-object.md#tostring) | `string` | Serialize to directive syntax |
