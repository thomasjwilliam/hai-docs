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

Trigger an action onclick, similar to the HTML [button](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/button) element.

```
{{button ...attributes}}
```

**Attributes**

`onclick` **required**\
The trigger of a snippet (to inject) or macro (to run), prefixed with the `$` dollar sign.

`text` **required**\
The text to be displayed

**Example**

```
{{button onclick="$MacroTriggerToRun" text="Click me"}}
```

### Checkbox group

The checkbox-group directive renders one or more child options as checkboxes. These checkboxes can be individually toggled, similar to the HTML [checkbox](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/input/checkbox) element.

**Syntax**

```
{{checkbox-group}}
  one or more {{option ...attributes}}
{{/checkbox-group}}
```

**Attributes**

`column`\
If defined, will display all checkboxes underneath each other.

`value`\
A comma-separated list of checked checkboxes value. Commas within values will be escaped by the system.

**Examples**

```
{{checkbox-group}}
  {{option value="foo"}}
  {{option value="bar" label="Bar Label"}}
  {{option value="baz"}}
{{/checkbox-group}}
```

```
{{checkbox-group value="foo,baz" column}}
  {{option value="foo"}}
  {{option value="bar" label="Bar Label"}}
  {{option value="baz"}}
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

`placeholder`\
The text displayed when there is no value.

`value`\
The value of the selected option or custom input.

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

`cols`\
A positive integer (default: `12`) or `auto` representing the visible width of the textarea, in average character widths.\
The value `auto` will set the width to 100% of the document.

`placeholder`\
The text displayed when there is no value.

`type`\
Possible values: `text` (default) | `date` | `datetime-local`

`value`\
The value of the input.

**Examples**

```
{{input}}
```

```
{{input type="text" placeholder="Write..."}}
```

```
{{input type="text" cols="auto" placeholder="Write..." value="Hello world!"}}
```

```
{{input type="date" value="1970-01-01"}}
```

```
{{input type="datetime-local" value="1970-01-01T00:00"}}
```

### Option

Provide an option to the `checkbox-group`, `datalist`, `radio-group` or `select` directives, similar to the HTML [option](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/option) element.

```
{{option ...attributes}}
```

**Attributes**

`value` **required**\
The value of the option

`label`\
The label to display for the option

**Examples**

```
{{option value="foo"}}
```

```
{{option value="bar" label="Bar label"}}
```

### Radio group

Select a single option within a group of options, similar to the HTML [input type="radio"](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/input/radio) element.

**Syntax**

```
{{radio-group}}
  two or more {{option ...attributes}}
{{/radio-group}}
```

**Attributes**

`column`\
If defined, will display all radios underneath each other.

`value`\
The value of the selected radio option. Any Commas within the value will be escaped by the system.

**Examples**

```
{{radio-group}}
  {{option value="foo"}}
  {{option value=“bar" label=“Bar Label"}}
  {{option value=“baz"}}
{{/radio-group}}
```

```
{{radio-group value=“bar" column}}
  {{option value=“foo"}}
  {{option value=“bar" label=“Bar Label"}}
  {{option value=“baz"}}
{{/radio-group}}
```

### Select

Select an option, similar to the HTML [select](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/select) element.

```
{{select ...attributes}}
  two or more {{option}} directives
{{/select}}
```

**Attributes**

`placeholder`\
The text displayed when there is no value.

`value`\
The value of the selected option.

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

`cols`\
A positive integer (default: `3`) representing the visible width of the textarea, in average character widths.

`placeholder`\
The text displayed when there is no value

`rows`\
A positive integer or `auto` (default) representing the visible height of the textarea.

`value`\
The textarea input.

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

Render a note.

```
{{note ...attributes}}
  any content
{{/note}}
```

**Attributes**


`collapsible`\
When defined, adds a button to collapse (fold) and expand (unfold) the note.

`confirmable`\
When defined, adds a button to confirm which will unwrap the note contents and replace it with those contents.

`dismissable`\
When defined, adds a button to dismiss the entire note, disregarding its contents.

`title`\
The title of the note.

`type`\
Value: `info` | `tip` | `success` | `warning` | `error`

**Examples**

```
{{note}}
  Simple note.
{{/note}}
```

```
{{note type="info"}}
  Informational note that can be folded.
{{/note}}
```

```
{{note type="success"}}
  Success that can be dismissed.
{{/note}}
```

```
{{note type="error" title="That's no good boss"}}
  Something happened.
{{/note}}
```

```
{{note collapsible confirmable dismissable}}
  ...
{{/note}}
```

### Section

```
{{section ...attributes}}
  any content
{{/section}}
```

**Attributes**

`collapsible`\
When defined, adds a button to collapse (fold) and expand (unfold) the section.

`confirmable`\
When defined, adds a button to confirm which will unwrap the section contents and replace it with those contents.

`dismissable`\
When defined, adds a button to dismiss the entire section, disregarding its contents.

`shaded`\
Provides a slight grey background color (and negative margin) to provide visual separation.

**Example**

```
{{section collapsible confirmable dismissable shaded}}
  ...
{{/section}}
```

## Global directives attributes

`class`
A [set of space-separated tokens](https://html.spec.whatwg.org/multipage/common-microsyntaxes.html#set-of-space-separated-tokens) that represents the various classes the directive belongs to, helpful for targeting them in macro scripts using the hai.doc utility library.

`id`
A completely unique identifier for a single element within a document, helpful for targeting them in macro scripts using the hai.doc utility library.
