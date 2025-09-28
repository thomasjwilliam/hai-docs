# Markdown directives

## About

Directives are special Markdown elements that are used to render content in the document.

## Form directives

### Button

**About**

Perform an action, similar to the HTML [button](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/button) element.

**Syntax**
```
{{ button ...attributes }}
```

**Attributes**

* `command`
* `text`

**Examples**

```
{{ button text="Click me" placeholder="SomeCommand" }}
```

### Input

**About**

Input some data, similar to the HTML [input](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/input) element.

**Syntax**
```
{{ input ...attributes }}
```

**Attributes**

* `cols`
* `placeholder`
* `type`: `text` (default) | `date` | `datetime-local`
* `value`

**Examples**

```
{{ input type="text" placeholder="Your name" }}
```

```
{{ input type="text" placeholder="Your name" value="J. Doe" }}
```

```
{{ input type="date" value="1970-01-01" }}
```

```
{{ input type="datetime-local" value="1970-01-01T00:00" }}
```

### Select

**About**

Select an option, similar to the HTML [select](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/select) element.

**Syntax**

```
{{ select ...attributes }}
```

**Attributes**

* `options`
* `placeholder`
* `selected`

**Examples**

```
{{ select options="foo,bar,baz" }}
```

```
{{ select options="foo(Foo label),bar(Bar label),baz(Baz label)" }}
```

```
{{ select options="foo(Foo label),bar(Bar label),baz(Baz label)" selected="bar" }}
```

### Datalist

**About**

Filter and/or select an option, similar to the HTML [datalist](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/datalist) element.

**Syntax**

```
{{ datalist ...attributes }}
```

**Attributes**

* `options`
* `placeholder`
* `value`

**Examples**

```
{{ datalist options="foo(foo),bar(bar),baz(baz)" }}
```

```
{{ datalist options="foo(Foo label),bar(Bar label),baz(Baz label)" }}
```

### Textarea

**About**

Write text, similar to the HTML [textarea](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/textarea) element.

**Syntax**

```
{{ textarea ...attributes }}
```

**Attributes**

* `cols`
* `value`
* `placeholder`
* `rows`

**Examples**

```
{{ textarea cols="20" rows="3" }}
```

```
{{ textarea placeholder="Enter text here..." cols="20" rows="3" }}
```

```
{{ textarea placeholder="Enter text here..." value="Predefined value" cols="20" rows="3" }}
```

## Sectioning directives

### Section

**About**

Define a section in the document, similar to the HTML [section](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/section) element.

**Syntax**

```
{{ section start ...attributes }}
whatever
{{ section end }}
```

**Attributes**

* `action`: `confirm` | `dismiss`
* `foldable`
* `title`
* `type`: `section` (default) | `info` | `success` | `warning` | `error`

**Examples**

```
{{ section start }}
Look at meeeee! I'm in my own little section.
{{ section end }}
```

```
{{ section start type="info" }}
Information.
{{ section end }}
```

```
{{ section start type="success" action="dismiss" }}
Success that can be dismissed.
{{ section end }}
```

```
{{ section start type="error" title="That's no good boss" foldable="true" }}
Something happened.
{{ section end }}
```

## Global directives attributes

* `class`
* `id`