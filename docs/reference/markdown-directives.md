# Markdown Directives

## About

Custom Markdown syntax to build interactive pages.

## Form directives

:::tip

Save a directive as a snippet.

Instead of writing it out each time, you can insert it with a click or /SnippetTrigger.\
For example, imagine a snippet `DatePicker`:
```
{{input type="date"}}
```

Now you can reference `$DatePicker` in other snippets or insert with a /DatePicker.

:::

### Button

Trigger an action, similar to the HTML [button](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/button) element.

```
{{button ...attributes}}
```

**Attributes**

* `text` (**required**)
  * The text to be displayed
* `trigger` (**required**)
  * The trigger of the macro to run when the button is clicked.

**Example**

```
{{button text="Click me" trigger="MacroTriggerToRun"}}
```

### Checkbox

Render a checkbox, similar to the HTML [checkbox](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/checkbox) element.

```
{{checkbox ...attributes}}
```

**Attributes**
* `name` (**required**)
  * A unique string to define the name of checkbox. If `label` is not defined, this will be rendered as the label.
* `checked`
  * if defined, indicates the option is selected. If the checkbox is checked via user interaction, this attribute is set. The attribute does not have a value. Only its attribute name.
* `label`
  * if defined, provides the checkbox label

**Examples**

```
{{checkbox name="foo"}}
```

```
{{checkbox name="bar" label="Bar Label" checked}}
```

### Checkbox group

Used to group a set of checkboxes, mostly for styling purposes.

```
{{checkbox-group ...attributes}}
  two or more {{checkbox ...attributes}} directives
{{/checkbox-group}}
```

**Attributes**
* `column`
  * if defined, will display all checkboxes under each other.

**Examples**

```
{{checkbox-group}}
  {{checkbox name="foo"}}
  {{checkbox name="bar" label="Bar Label"}}
{{/checkbox-group}}
```

```
{{checkbox-group column}}
 {{checkbox name="foo"}}
 {{checkbox name="bar" label="Bar Label"}}
{{/checkbox-group}}
```

### Datalist

Type to filter and/or select an option, or completely custom input, similar to the HTML [datalist](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/datalist) element.

```
{{datalist ...attributes}}
  with two or more {{option}} directives
{{/datalist}}
```

**Attributes**

* `placeholder`
  * The text displayed when there is no value.
* `value`
  * The value of the selected option or custom input.

**Example**

```
{{datalist placeholder="Your Job Title:"}}
  {{option value="Software Engineer"}}
  {{option value="Product Manager"}}
  {{option value="UX/UI Designer"}}
{{/datalist}}
```

### Input

Input some text, similar to the HTML [input](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/input) element.

```
{{input ...attributes}}
```

**Attributes**

* `cols`
  * The number of characters to set the width of the input, `auto` to set the width to 100% of the document.
  * Value: number | `auto`
  * Default: `12`
* `placeholder`
  * The text displayed when there is no value.
* `type`
  * Value: `text` | `date` | `datetime-local`
  * Default: `text`
* `value`

**Examples**

```
{{input}}
```

```
{{input type="text" placeholder="Write..."}}
```

```
{{input type="text" placeholder="Write..." value="Hello world!" cols="auto"}}
```

```
{{input type="date" value="1970-01-01"}}
```

```
{{input type="datetime-local" value="1970-01-01T00:00"}}
```

### Option

Provide an option to the `datalist` or `select` directives, similar to the HTML [option](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/option) element.

```
{{option ...attributes}}
```

**Attributes**

* `value` (**required**)
  * The value of the option
* `label`
  * The label to display for the option

**Examples**

```
{{option value="foo"}}
```

```
{{option value="bar" label="Bar label"}}
```

### Radio

Select a single option within a group of options, similar to the HTML [<input type="radio">](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/input/radio) element.

**Syntax**

```
{{radio ...attributes}}
  One or more {{option}} directives
{{/radio}}
```

**Attributes**

* `name`
  * A unique string to define the radio group
* `value`
  * The value of the selected option.

**Examples**

```
{{radio}}
  {{option value="foo”}}
  {{option value=“bar” label=“Bar Label”}}
  {{option value=“baz”}}
{{/radio}}
```

```
{{radio value=“bar”}}
  {{option value=“foo”}}
  {{option value=“bar” label=“Bar Label”}}
  {{option value=“baz”}}
{{/radio}}
```

### Select

Select an option, similar to the HTML [select](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/select) element.

```
{{select ...attributes}}
  with two or more {{option}} directives
{{/select}}
```

**Attributes**

* `placeholder`
  * The text displayed when there is no value.
* `value`
  * The value of the selected option.

**Examples**

```
{{select placeholder="Select a T-shirt size:"}}
  {{option value="s" label="Small"}}
  {{option value="m" label="Medium"}}
  {{option value="l" label="Large"}}
  {{option value="xl" label="Extra Large"}}
{{/select}}
```

```
{{select placeholder="Notification Frequency" value="Weekly"}}
  {{option value="Daily"}}
  {{option value="Weekly"}}
  {{option value="Monthly"}}
{{/select}}
```

### Textarea

Write text, similar to the HTML [textarea](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/textarea) element.

```
{{textarea ...attributes}}
```

**Attributes**

* `rows`
  * The height of the textarea.
  * Value: `auto` | number
  * Default: `auto`
* `placeholder`
  * The text displayed when there is no value
* `rows`
  * The number of characters to set the width of the textarea.
  * Value: number
  * Default: `3`
* `value`
  * The textarea input.

**Examples**

```
{{textarea}}
```

```
{{textarea placeholder="Write..."}}
```

```
{{textarea placeholder="Write..." value="Hello world!"}}
```

```
{{textarea placeholder="Write..." cols="60" rows="6"}}
```

## Informational and sectioning directives

### Note

Provides an note.

```
{{note ...attributes}}
  any content
{{/note}}
```

**Attributes**

* `action`
  * When defined, adds a button to `confirm` or `dimiss` the note. When confirmed, its contents will be unwrapped and replace the note. If dismissed, the entire note will be removed.
  * Value: `confirm` | `dismiss`
* `foldable`
  * When defined, adds a button to fold and unfold the note.
* `title`
  * The title of the note.
* `type`
  * Value: `info` | `tip` | `success` | `warning` | `error`

**Examples**

```
{{note}}
  Simple note.
{{/note}}
```

```
{{note type="info" foldable="true"}}
  Informational note that can be folded.
{{/note}}
```

```
{{note type="success" action="dismiss"}}
  Success that can be dismissed.
{{/note}}
```

```
{{note type="error" title="That's no good boss"}}
  Something happened.
{{/note}}
```

### Section

```
{{section ...attributes}}
  any content
{{/section}}
```

**Attributes**

* `action`
  * When defined, adds a button to `confirm` or `dimiss` the section. When confirmed, its contents will be unwrapped and replace the section. If dismissed, the entire section will be removed.
  * Value: `confirm` | `dismiss`
* `foldable`
  * When defined, adds a button to fold and unfold the note.
* `background-color`
  * Provides a slight grey background color (and negative margin) to provide visual separation.

**Example**

```
{{section action="dismiss" foldable="true" background-color}}
#### This section can be:
- folded, or
- dismissed
{{/section}}
```

## Global directives attributes

* `class`
  * The class attribute assigns a directive to a named group, acting as a selector to programmatically target and manipulate a set of directives with the [`hai.doc` utility library](https://thomasjwilliam.github.io/hai-docs/docs/reference/macro-utility-library/).
  * Value: one or more names separated by whitespace.
* `id`
  * The id attribute provides a completely unique identifier for a single element within a document that can be targetted and manipulated with the [`hai.doc` utility library](https://thomasjwilliam.github.io/hai-docs/docs/reference/macro-utility-library/).
  * Value: a single unique name.
