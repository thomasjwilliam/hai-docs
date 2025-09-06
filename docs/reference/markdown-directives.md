# Markdown directives

## Input

**About**

Input some data, similar to the HTML [input](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/input) element.

**Syntax**

```
{{ input ...attributes }}
```

**Attributes**

* `class`
* `cols`
* `id`
* `placeholder`
* `type`: `text` (default) | `date` | `datetime-local`
* `value`

**Examples**

```
{{ input type="text" placeholder="Your name" }}
```

```
{{ input type="text" placeholder="Your name" value="J Doe" }}
```

```
{{ input type="date" value="1970-01-01" }}
```

```
{{ input type="datetime-local" value="1970-01-01T00:00" }}
```

## Select

**About**

Select an option, similar to the HTML [select](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/select) element.

**Syntax**

```
{{ select ...attributes }}
```

**Attributes**

* `class`
* `id`
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

## datalist

**About**

Filter and/or select an option, similar to the HTML [datalist](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/datalist) element.

**Syntax**

```
{{ datalist ...attributes }}
```

**Attributes**

* `class`
* `id`
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

## Textarea

**About**

Write text, similar to the HTML [textarea](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/textarea) element.

**Syntax**

```
{{ textarea ...attributes }}
```

**Attributes**

* `class`
* `cols`
* `id`
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

## Section

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
* `class`
* `foldable`
* `id`
* `title`
* `type`: `section` (default) | `info` | `success` | `warning` | `error`

**Examples**

```
{{ section start }}
Look at meeeee! I'm in my own little section.

{{ section end }}
```

```
{{ section start type="error" title="That's no good boss" }}
Something happened.

{{ section end }}
```

```
{{ section start type="success" action="dismiss" }}
Success that can be dismissed.

{{ section end }}
```

```
{{ section start type="info" action="dismiss" foldable="true" }}
Information that can be folded or dismissed.

{{ section end }}
```