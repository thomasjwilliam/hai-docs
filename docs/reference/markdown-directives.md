# Markdown Directives

## About

Directives are custom Markdown elements that are used to render interactive elements in the document.

## Form directives

### Button

Perform an action, similar to the HTML [button](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/button) element.

**Syntax**
```
{{button ...attributes}}
```

**Attributes**

* `command`: the macro trigger to run when the button is clicked
* `text`: the text to display on the button

**Examples**

```
{{button text="Click me" command="MacroTrigger"}}
```

### Datalist

Filter and/or select an option, similar to the HTML [datalist](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/datalist) element.

**Syntax**

```
{{datalist ...attributes}}
... with one or more {{option ...attributes}} directives
{{/datalist}}
```

**Attributes**

* `name`
* `placeholder`
* `value`

**Examples**

```
{{datalist}}
{{option value="foo"}}
{{option value="bar"}}
{{option value="baz"}}
{{/datalist}}
```

```
{{datalist value="bar"}}
{{option value="foo" label="Foo Label"}}
{{option value="bar" label="Bar Label"}}
{{option value="baz" label="Baz Label"}}
{{/datalist}}
```

### Input

Input some data, similar to the HTML [input](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/input) element.

**Syntax**
```
{{input ...attributes}}
```

**Attributes**

* `cols`: the number of columns to define the width
* `placeholder`
* `type`: `text` (default) | `date` | `datetime-local`
* `value`

**Examples**

```
{{input type="text" placeholder="Your name"}}
```

```
{{input type="text" placeholder="Your name" value="J. Doe"}}
```

```
{{input type="date" value="1970-01-01"}}
```

```
{{input type="datetime-local" value="1970-01-01T00:00"}}
```

### Option

Provide an option to the `datalist` or `select` directives, similar to the HTML [option](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/option) element.

**Syntax**
```
{{option ...attributes}}
```

**Attributes**

* `label`: the label to display for the option
* `value`

**Examples**

```
{{option value="foo"}}
```

```
{{option value="bar" label="Bar label"}}
```

### Select

Select an option, similar to the HTML [select](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/select) element.

**Syntax**

```
{{select ...attributes}}
... with one or more {{option ...attributes}} directives
{{/select}}
```

**Attributes**

* `name`
* `placeholder`
* `value`

**Examples**

```
{{select}}
{{option value="foo"}}
{{option value="bar"}}
{{option value="baz"}}
{{/select}}
```

```
{{select}}
{{option value="foo" label="Foo Label"}}
{{option value="bar" label="Bar Label"}}
{{option value="baz" label="Baz Label"}}
{{/select}}
```

```
{{select value="bar"}}
{{option value="foo" label="Foo Label"}}
{{option value="bar" label="Bar Label"}}
{{option value="baz" label="Baz Label"}}
{{/select}}
```

### Textarea

Write text, similar to the HTML [textarea](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/textarea) element.

**Syntax**

```
{{textarea ...attributes}}
```

**Attributes**

* `cols`the number of columns to define the width
* `value`
* `placeholder`
* `rows`

**Examples**

```
{{textarea cols="20" rows="3"}}
```

```
{{textarea placeholder="Enter text here..." cols="20" rows="3"}}
```

```
{{textarea placeholder="Enter text here..." value="Predefined value" cols="20" rows="3"}}
```

## Informational and sectioning directives

### Note

**Syntax**

```
{{note ...attributes}}
... any content comes here
{{/note}}
```

**Attributes**

* `action`: `confirm` | `dismiss`
* `foldable`
* `title`
* `type`: `info` | `tip` | `success` | `warning` | `error`

**Examples**

```
{{note type="info"}}
Information.
{{/note}}
```

```
{{note type="success" action="dismiss"}}
Success that can be dismissed.
{{/note}}
```

```
{{note type="error" title="That's no good boss" foldable="true"}}
Something happened.
{{/note}}
```

### Section

**Syntax**

```
{{section ...attributes}}
... any content comes here
{{/section}}
```

**Attributes**

* `action`: `confirm` | `dismiss`
* `foldable`

**Examples**

```
{{section}}
...
{{/section}}
```

```
{{section action="dismiss"}}
...
{{/section}}
```

```
{{section foldable="true"}}
...
{{/section}}
```

## Global directives attributes

* `class`
* `id`