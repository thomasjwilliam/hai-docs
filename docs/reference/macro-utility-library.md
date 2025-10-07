# Macro Utility Library

The global `hai` library exposes `app` and `doc` utility functions.

```
hai = {
  app,
  doc
}
```

:::tip

Use object destructuring:

```javascript
const {app, doc} = hai;

app.notify({
    type: "info", 
    message: "JS destructuring for the win!",
    sticky: true
});

doc.setContent("I'm a JS ninja");
```

:::

## hai.app

### notify

Displays a notification to the user.

```
notify(notification: {
    message: string
    type: "info" | "success" | "warning" | "error"
    sticky?: boolean
    title?: string
})
```

## hai.doc

A set of handy functions to work with the document and its directives. Many of these functions return a `DirectiveObject` (or an array of them), which allows for powerful manipulation of directives.

### getContent
```
getContent(): string
```

### getDirectiveByAttVal
Finds the first directive object that has an attribute with the specified name and value.
```
getDirectiveByAttVal(att: string, val: string, dir?: string): DirectiveObject | undefined
```

### getDirectiveById
Finds a directive object by its `id` attribute.
```
getDirectiveById(id: string, dir?: string): DirectiveObject | undefined
```

### getDirectivesByAttVal
Finds all directive objects that have an attribute with the specified name and value.
```
getDirectivesByAttVal(att: string, val: string, dir?: string): DirectiveObject[]
```

### getDirectivesByClass
Finds all directive objects that have a specified class name.
```
getDirectivesByClass(class: string, dir?: string): DirectiveObject[]
```

### getDirectivesByTagName
Finds all directive objects that match the given tag name.
```
getDirectivesByTagName(tagName: string, dir?: string): DirectiveObject[]
```

### getTitle
```
getTitle(): string | undefined
```

### setContent
```
setContent(content: string): void
```

### setTitle
```
setTitle(title: string): void
```

## DirectiveObject

Represents a directive in the document, providing methods to read and manipulate it. Attributes of the directive can be accessed and modified directly as properties of the object.

**Example**

```javascript
// Given a directive string: '{{note id="some-id" dismissable}}...{{/note}}'
const dir = doc.getDirectiveById('some-id');

// Getting attributes
console.log(dir.id); // Outputs: 'some-id'
console.log(dir.dismissable); // Outputs: true (for value-less attributes)

// Setting an attribute
dir.class = 'some-class'; // Updates the directive to '{{note class="some-class" id="some-id" dismissable}}...{{/note}}}}'

// Adding a value-less attribute
dir.confirmable = true; // Updates the directive to '{{... confirmable dismissable}}'

// Removing an attribute
delete dir.id; // Updates the directive to '{{note dismissable}}...{{/note}}'
```

### Properties

#### tagName
The tag name of the directive.
```
readonly tagName: string
```

### Methods

#### getDirectiveByAttVal
Searches within the inner content of the current directive for the first nested directive that has an attribute with the specified name and value.
```
getDirectiveByAttVal(att: string, val: string): undefined | DirectiveObject
```

#### getDirectiveById
Searches within the inner content of the current directive for a nested directive by its `id` attribute.
```
getDirectiveById(id: string): undefined | DirectiveObject
```

#### getDirectivesByAttVal
Searches within the inner content of the current directive for all nested directives that have an attribute with the specified name and value.
```
getDirectivesByAttVal(att: string, val: string): DirectiveObject[]
```

#### getDirectivesByClass
Searches within the inner content of the current directive for all nested directives that have a specified class name.
```
getDirectivesByClass(cls: string): DirectiveObject[]
```

#### getDirectivesByTagName
Searches within the inner content of the current directive for all nested directives that match the given tag name.
```
getDirectivesByTagName(tagName: string): DirectiveObject[]
```

#### insertAfter
Inserts a new directive immediately after the current directive.
```
insertAfter(dir: DirectiveObject | string): void
```

#### insertBefore
Inserts a new directive immediately before the current directive.
```
insertBefore(dir: DirectiveObject | string): void
```

#### remove
Removes the directive from the document. The current `DirectiveObject` instance will become invalid after this operation.
```
remove(): void
```

#### replaceWith
Replaces the current directive with a new directive. The current `DirectiveObject` instance will be updated to represent the new content.
```
replaceWith(dir: DirectiveObject | string): void
```

#### toString
Returns the string representation of the directive.
```
toString(): string
```
