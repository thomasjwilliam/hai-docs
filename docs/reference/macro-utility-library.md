# Macro Utility Library

The global `hai` library provides utilities for working with document content and displaying notifications within macros. It exposes two main utility objects: `app` and `doc`.

```javascript
hai = {
  app,  // Application utilities (notifications)
  doc   // Document manipulation utilities
}
```

## Quick Start

:::tip

Use object destructuring for cleaner code:

```javascript
const {app, doc} = hai;

// Display a notification
app.notify({
  type: "success", 
  message: "Task completed!",
  sticky: true
});

// Update document content
doc.setContent("New content here");

// Find and modify directives
const myDiv = doc.getDirectiveById('my-div');
myDiv.class = 'highlight';
myDiv.innerText = 'Updated text';
```

:::

## Understanding Directives

Directives are template elements in your document with the syntax:
```
{{tagName attr="value" booleanAttr}}content{{/tagName}}
```

Self-closing directives (input, textarea, button, etc) don't have closing tags:
```
{{input type="text" id="name"}}
```

**Examples:**
```javascript
{{div id="container" class="main"}}
  Some content here
{{/div}}

{{note remove}}Important note{{/note}}

{{input type="text" placeholder="Enter name"}}
```

---

## hai.app

Application-level utilities for user interactions.

### notify

Displays a notification to the user.

**Signature:**
```typescript
notify(notification: {
  message: string
  type: "info" | "success" | "warning" | "error"
  sticky?: boolean
  title?: string
}): void
```

**Parameters:**
- `message` (string, required) - The main notification message
- `type` (string, required) - Notification type: "info", "success", "warning", or "error"
- `sticky` (boolean, optional) - If true, notification remains until manually removeed
- `title` (string, optional) - Optional title for the notification

**Examples:**

```javascript
// Simple info notification
app.notify({
  type: "info",
  message: "Processing started..."
});

// Success notification with title
app.notify({
  type: "success",
  title: "Complete!",
  message: "All tasks have been processed",
  sticky: false
});

// Sticky warning that requires removeal
app.notify({
  type: "warning",
  message: "Please review the changes before saving",
  sticky: true
});

// Error notification
app.notify({
  type: "error",
  title: "Error",
  message: "Failed to save document"
});
```

---

## hai.doc

Utilities for manipulating document content and directives. Methods return `DirectiveObject` instances that provide powerful DOM-like manipulation capabilities.

### createDirective

Creates a new directive that can be inserted into the document.

**Signature:**
```typescript
createDirective(localName: string): DirectiveObject
```

**Parameters:**
- `localName` (string) - The tag name for the directive (e.g., 'div', 'note', 'input')

**Returns:** A new `DirectiveObject` instance

**Note:** Self-closing directives (input, textarea, button, option) are automatically created without closing tags.

**Examples:**

```javascript
// Create a div directive
const newDiv = doc.createDirective('div');
newDiv.id = 'my-div';
newDiv.class = 'container';
newDiv.innerText = 'Hello World';

// Insert it into the document
const existingDiv = doc.getDirectiveById('parent');
existingDiv.insertAfter(newDiv);

// Create a self-closing input directive
const input = doc.createDirective('input');
input.type = 'text';
input.placeholder = 'Enter your name';
```

### getContent

Gets the current document content.

**Signature:**
```typescript
getContent(): string
```

**Returns:** The document content as a string

**Example:**

```javascript
const content = doc.getContent();
console.log('Document has', content.length, 'characters');
```

### setContent

Sets the document content, replacing all existing content.

**Signature:**
```typescript
setContent(content: string): void
```

**Parameters:**
- `content` (string) - The new content for the document

**Example:**

```javascript
doc.setContent('{{div}}New document content{{/div}}');
```

### getTitle

Gets the current document title.

**Signature:**
```typescript
getTitle(): string
```

**Returns:** The document title

**Example:**

```javascript
const title = doc.getTitle();
console.log('Document title:', title);
```

### setTitle

Sets the document title.

**Signature:**
```typescript
setTitle(title: string): void
```

**Parameters:**
- `title` (string) - The new title for the document

**Example:**

```javascript
doc.setTitle('My Updated Document');
```

### getDirectiveById

Finds a directive by its `id` attribute.

**Signature:**
```typescript
getDirectiveById(id: string): DirectiveObject | undefined
```

**Parameters:**
- `id` (string) - The id to search for

**Returns:** The matching `DirectiveObject` or `undefined` if not found

**Example:**

```javascript
const container = doc.getDirectiveById('main-container');
if (container) {
  container.class = 'active';
}
```

### getDirectiveByAttVal

Finds the first directive with a specific attribute value.

**Signature:**
```typescript
getDirectiveByAttVal(att: string, val: string): DirectiveObject | undefined
```

**Parameters:**
- `att` (string) - The attribute name to search for
- `val` (string) - The attribute value to match

**Returns:** The matching `DirectiveObject` or `undefined` if not found

**Example:**

```javascript
// Find first directive with data-type="important"
const important = doc.getDirectiveByAttVal('data-type', 'important');
if (important) {
  console.log('Found:', important.tagName);
}
```

### getDirectivesByAttVal

Finds all directives with a specific attribute value.

**Signature:**
```typescript
getDirectivesByAttVal(att: string, val: string): DirectiveObject[]
```

**Parameters:**
- `att` (string) - The attribute name to search for
- `val` (string) - The attribute value to match

**Returns:** Array of matching `DirectiveObject` instances

**Example:**

```javascript
// Find all directives with status="pending"
const pending = doc.getDirectivesByAttVal('status', 'pending');
console.log(`Found ${pending.length} pending items`);

pending.forEach(item => {
  item.status = 'completed';
});
```

### getDirectivesByClass

Finds all directives with a specific CSS class name.

**Signature:**
```typescript
getDirectivesByClass(cls: string): DirectiveObject[]
```

**Parameters:**
- `cls` (string) - The class name to search for

**Returns:** Array of matching `DirectiveObject` instances

**Note:** This searches for the class name within space-separated class lists.

**Example:**

```javascript
// Find all directives with class "highlight"
const highlighted = doc.getDirectivesByClass('highlight');

// Remove highlight from all
highlighted.forEach(item => {
  item.class = item.class.replace('highlight', '').trim();
});
```

### getDirectivesByTagName

Finds all directives with a specific tag name.

**Signature:**
```typescript
getDirectivesByTagName(tagName: string): DirectiveObject[]
```

**Parameters:**
- `tagName` (string) - The tag name to search for (e.g., 'div', 'note')

**Returns:** Array of matching `DirectiveObject` instances

**Example:**

```javascript
// Find all note directives
const notes = doc.getDirectivesByTagName('note');
console.log(`Document contains ${notes.length} notes`);

// Add remove attribute to all notes
notes.forEach(note => {
  note.remove = true;
});
```

---

## DirectiveObject

Represents a directive in the document, providing methods to read and manipulate it. Attributes can be accessed and modified directly as object properties.

### Properties

#### tagName

The tag name of the directive (read-only).

**Type:** `string` (readonly)

**Example:**

```javascript
const dir = doc.getDirectiveById('my-div');
console.log(dir.tagName); // Outputs: "div"
```

#### innerText

The text content inside the directive. Returns `undefined` for self-closing directives.

**Type:** `string | undefined`

**Examples:**

```javascript
// Reading innerText
const note = doc.getDirectiveById('my-note');
console.log(note.innerText); // Outputs the text content

// Setting innerText
note.innerText = 'Updated note content';

// Self-closing directives
const input = doc.getDirectivesByTagName('input')[0];
console.log(input.innerText); // Outputs: undefined

// Attempting to set innerText on self-closing throws error
// input.innerText = 'text'; // Error: Cannot set innerText on self-closing directive
```

#### Dynamic Attribute Access

You can access and modify any directive attribute directly as a property:

**Examples:**

```javascript
const dir = doc.getDirectiveById('example');

// Reading attributes
console.log(dir.id);        // "example"
console.log(dir.class);     // "container main"
console.log(dir.disabled);  // true (for boolean attributes)

// Setting attributes
dir.class = 'new-class';
dir.title = 'Hover text';
dir['data-value'] = '123';

// Boolean attributes
dir.disabled = true;   // Adds disabled attribute
dir.readonly = true;   // Adds readonly attribute

// Removing attributes
delete dir.class;      // Removes class attribute
dir.disabled = false;  // Removes disabled attribute
```

### Methods

#### getAttribute

Gets the value of a specified attribute.

**Signature:**
```typescript
getAttribute(name: string): string | null
```

**Parameters:**
- `name` (string) - The attribute name

**Returns:** The attribute value, empty string for boolean attributes, or `null` if not found

**Example:**

```javascript
const dir = doc.getDirectiveById('my-div');

const id = dir.getAttribute('id');        // "my-div"
const cls = dir.getAttribute('class');    // "container"
const disabled = dir.getAttribute('disabled'); // "" (empty string for boolean)
const missing = dir.getAttribute('nonexistent'); // null
```

#### setAttribute

Sets or removes an attribute.

**Signature:**
```typescript
setAttribute(name: string, val: string | boolean): void
```

**Parameters:**
- `name` (string) - The attribute name
- `val` (string | boolean) - The value to set. Use `true` for boolean attributes, `false`/`null`/`undefined` to remove

**Example:**

```javascript
const dir = doc.getDirectiveById('my-div');

// Set string attribute
dir.setAttribute('class', 'container');
dir.setAttribute('data-value', '123');

// Set boolean attribute
dir.setAttribute('disabled', true);

// Remove attribute
dir.setAttribute('disabled', false);
dir.setAttribute('class', null);
```

#### getDirectiveById

Searches within the current directive's content for a nested directive by id.

**Signature:**
```typescript
getDirectiveById(id: string): DirectiveObject | undefined
```

**Parameters:**
- `id` (string) - The id to search for

**Returns:** The matching `DirectiveObject` or `undefined` if not found

**Example:**

```javascript
const container = doc.getDirectiveById('container');
const nested = container.getDirectiveById('nested-item');
```

#### getDirectiveByAttVal

Searches within the current directive's content for the first nested directive with a specific attribute value.

**Signature:**
```typescript
getDirectiveByAttVal(att: string, val: string): DirectiveObject | undefined
```

**Parameters:**
- `att` (string) - The attribute name to search for
- `val` (string) - The attribute value to match

**Returns:** The matching `DirectiveObject` or `undefined` if not found

**Example:**

```javascript
const form = doc.getDirectiveById('my-form');
const submitBtn = form.getDirectiveByAttVal('type', 'submit');
```

#### getDirectivesByAttVal

Searches within the current directive's content for all nested directives with a specific attribute value.

**Signature:**
```typescript
getDirectivesByAttVal(att: string, val: string): DirectiveObject[]
```

**Parameters:**
- `att` (string) - The attribute name to search for
- `val` (string) - The attribute value to match

**Returns:** Array of matching `DirectiveObject` instances

**Example:**

```javascript
const container = doc.getDirectiveById('task-list');
const pending = container.getDirectivesByAttVal('status', 'pending');
```

#### getDirectivesByClass

Searches within the current directive's content for all nested directives with a specific class name.

**Signature:**
```typescript
getDirectivesByClass(cls: string): DirectiveObject[]
```

**Parameters:**
- `cls` (string) - The class name to search for

**Returns:** Array of matching `DirectiveObject` instances

**Example:**

```javascript
const section = doc.getDirectiveById('main-section');
const alerts = section.getDirectivesByClass('alert');
```

#### getDirectivesByTagName

Searches within the current directive's content for all nested directives with a specific tag name.

**Signature:**
```typescript
getDirectivesByTagName(tagName: string): DirectiveObject[]
```

**Parameters:**
- `tagName` (string) - The tag name to search for

**Returns:** Array of matching `DirectiveObject` instances

**Example:**

```javascript
const form = doc.getDirectiveById('my-form');
const inputs = form.getDirectivesByTagName('input');
inputs.forEach(input => {
  input.required = true;
});
```

#### insertAfter

Inserts a directive or string immediately after the current directive.

**Signature:**
```typescript
insertAfter(dir: DirectiveObject | string): void
```

**Parameters:**
- `dir` (DirectiveObject | string) - The directive to insert

**Example:**

```javascript
const div = doc.getDirectiveById('my-div');

// Insert a DirectiveObject
const newNote = doc.createDirective('note');
newNote.innerText = 'Added after';
div.insertAfter(newNote);

// Insert a string
div.insertAfter('{{div}}Another div{{/div}}');
```

#### insertBefore

Inserts a directive or string immediately before the current directive.

**Signature:**
```typescript
insertBefore(dir: DirectiveObject | string): void
```

**Parameters:**
- `dir` (DirectiveObject | string) - The directive to insert

**Example:**

```javascript
const div = doc.getDirectiveById('my-div');

// Insert a DirectiveObject
const newNote = doc.createDirective('note');
newNote.innerText = 'Added before';
div.insertBefore(newNote);

// Insert a string
div.insertBefore('{{div}}Another div{{/div}}');
```

#### remove

Removes the directive from the document.

**Signature:**
```typescript
remove(): void
```

**Warning:** The `DirectiveObject` instance becomes invalid after removal.

**Example:**

```javascript
const oldDiv = doc.getDirectiveById('obsolete');
if (oldDiv) {
  oldDiv.remove();
  // oldDiv is now invalid - don't use it further
}
```

#### replaceWith

Replaces the current directive with another directive or string.

**Signature:**
```typescript
replaceWith(dir: DirectiveObject | string): void
```

**Parameters:**
- `dir` (DirectiveObject | string) - The directive to replace with

**Example:**

```javascript
const oldDiv = doc.getDirectiveById('old-div');

// Replace with a DirectiveObject
const newDiv = doc.createDirective('div');
newDiv.id = 'new-div';
newDiv.innerText = 'Replaced content';
oldDiv.replaceWith(newDiv);

// Replace with a string
oldDiv.replaceWith('{{note}}This replaces the old div{{/note}}');
```

#### toString

Returns the string representation of the directive.

**Signature:**
```typescript
toString(): string
```

**Returns:** The directive as a string in the format `{{tagName attr="value"}}...{{/tagName}}`

**Example:**

```javascript
const div = doc.getDirectiveById('my-div');
console.log(div.toString());
// Outputs: {{div id="my-div" class="container"}}Content{{/div}}

// Useful for debugging or logging
const allNotes = doc.getDirectivesByTagName('note');
allNotes.forEach(note => {
  console.log(note.toString());
});
```

---

## Advanced Usage

### Creating and Inserting Directives

```javascript
// Create a new note directive
const note = doc.createDirective('note');
note.id = 'important-note';
note.class = 'highlight warning';
note.remove = true;
note.innerText = 'This is an important reminder!';

// Insert it after an existing directive
const section = doc.getDirectiveById('main-section');
section.insertAfter(note);
```

### Working with Nested Directives

```javascript
// Find a container and work with its children
const taskList = doc.getDirectiveById('task-list');

// Find all completed tasks within the list
const completed = taskList.getDirectivesByAttVal('status', 'done');
console.log(`Completed tasks: ${completed.length}`);

// Archive completed tasks by adding a class
completed.forEach(task => {
  task.class = (task.class || '') + ' archived';
});
```

### Manipulating Classes

```javascript
const element = doc.getDirectiveById('my-element');

// Get current classes
const classes = element.class ? element.class.split(' ') : [];

// Add a class
if (!classes.includes('active')) {
  classes.push('active');
  element.class = classes.join(' ');
}

// Remove a class
const filtered = classes.filter(c => c !== 'inactive');
element.class = filtered.join(' ');

// Toggle a class
const hasHighlight = classes.includes('highlight');
if (hasHighlight) {
  element.class = classes.filter(c => c !== 'highlight').join(' ');
} else {
  classes.push('highlight');
  element.class = classes.join(' ');
}
```

### Updating Multiple Directives

```javascript
// Find all input directives and make them required
const inputs = doc.getDirectivesByTagName('input');
inputs.forEach(input => {
  input.required = true;
  input.class = 'form-control';
});

// Update all notes with a specific status
const pendingNotes = doc.getDirectivesByAttVal('status', 'pending');
pendingNotes.forEach(note => {
  note.status = 'reviewed';
  note.class = 'reviewed';
});
```

### Building Complex Structures

```javascript
// Create a form dynamically
const form = doc.createDirective('div');
form.id = 'user-form';
form.class = 'form-container';

// Create and add input fields
const nameInput = doc.createDirective('input');
nameInput.type = 'text';
nameInput.id = 'name';
nameInput.placeholder = 'Enter your name';

const emailInput = doc.createDirective('input');
emailInput.type = 'email';
emailInput.id = 'email';
emailInput.placeholder = 'Enter your email';

// Build the structure using string concatenation
form.innerText = nameInput.toString() + '\n' + emailInput.toString();

// Insert the form into the document
const container = doc.getDirectiveById('main-container');
container.insertAfter(form);
```

---

## Common Pitfalls

### Self-Closing Directives

**Problem:** Attempting to set `innerText` on self-closing directives throws an error.

```javascript
// ❌ This will throw an error
const input = doc.createDirective('input');
input.innerText = 'text'; // Error!

// ✅ Self-closing directives don't have innerText
const input = doc.createDirective('input');
input.placeholder = 'Use attributes instead';
```

### Boolean Attributes

**Problem:** Confusion about boolean attribute values.

```javascript
// Boolean attributes are set with true
dir.disabled = true;    // ✅ Adds disabled attribute
dir.readonly = true;    // ✅ Adds readonly attribute

// Set to false/null/undefined to remove
dir.disabled = false;   // ✅ Removes disabled attribute
delete dir.disabled;    // ✅ Also removes disabled attribute

// Getting boolean attributes
console.log(dir.disabled); // true if present, undefined if not
console.log(dir.getAttribute('disabled')); // "" if present, null if not
```

### Working with Removed Directives

**Problem:** Using a `DirectiveObject` after calling `remove()`.

```javascript
const div = doc.getDirectiveById('my-div');
div.remove();
// ❌ Don't use div anymore - it's invalid
// div.class = 'something'; // This won't work as expected
```

### Class Attribute Manipulation

**Problem:** Not handling space-separated class names correctly.

```javascript
// ❌ This overwrites all classes
dir.class = 'new-class';

// ✅ Append to existing classes
const classes = dir.class ? dir.class.split(' ') : [];
classes.push('new-class');
dir.class = classes.join(' ');

// ✅ Or use string concatenation
dir.class = (dir.class || '') + ' new-class';
```

---

## Best Practices

1. **Check for existence before manipulating:**
   ```javascript
   const element = doc.getDirectiveById('my-id');
   if (element) {
     element.class = 'active';
   }
   ```

2. **Use appropriate search methods:**
    - Use `getDirectiveById()` for unique elements
    - Use `getDirectivesByTagName()` to find all elements of a type
    - Use `getDirectivesByClass()` for elements sharing a class
    - Use `getDirectivesByAttVal()` for custom attributes

3. **Prefer property access for simple operations:**
   ```javascript
   // ✅ Simple and readable
   dir.id = 'new-id';
   dir.class = 'container';
   
   // Also valid but more verbose
   dir.setAttribute('id', 'new-id');
   dir.setAttribute('class', 'container');
   ```

4. **Use `createDirective()` for dynamic content:**
   ```javascript
   const newDiv = doc.createDirective('div');
   newDiv.id = 'dynamic-' + Date.now();
   newDiv.innerText = 'Dynamic content';
   ```

5. **Always handle array results:**
   ```javascript
   const items = doc.getDirectivesByClass('item');
   if (items.length > 0) {
     items.forEach(item => {
       // Process each item
     });
   }
