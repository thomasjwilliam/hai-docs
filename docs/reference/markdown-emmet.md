# Emmet Abbreviation Syntax

HAI supports a subset of [Emmet](https://emmet.io) abbreviation syntax in the Markdown editor. Type an abbreviation and press **Tab** to expand it into a HAI directive or CommonMark markup.

Output is not HTML — it is either a **HAI directive** (`{{tag ...}}` / `{{/tag}}`) or **CommonMark markdown** (headings, lists, plain text), depending on the element.

---

## Elements

Abbreviations start with a tag name. Only recognised HAI directive tags are valid:

`div`, `note`, `span`, `input`, `textarea`, `button`, `select`, `datalist`, `option`, `checkbox-group`, `radio-group`, `p`, `h1`–`h6`, `ul`, `ol`, `li`

**`div`**

```
{{div}}
{{/div}}
```

**`input`**

```
{{input}}
```

**`textarea`**

```
{{textarea}}
{{/textarea}}
```

If the abbreviation starts with `.`, `#`, or `[` (no explicit tag), the tag defaults to `div`.

**`.panel`**

```
{{div class="panel"}}
{{/div}}
```

**`#main`**

```
{{div id="main"}}
{{/div}}
```

**`[aria-label="main"]`**

```
{{div aria-label="main"}}
{{/div}}
```

---

## Nesting Operators

### Child `>`

Nests elements inside each other.

**`select>option`**

```
{{select}}
  {{option}}
{{/select}}
```

**`div>select>option`**

```
{{div}}
  {{select}}
    {{option}}
  {{/select}}
{{/div}}
```

### Sibling `+`

Places elements at the same level.

**`button{Save}+button{Cancel}`**

```
{{button text="Save"}}
{{button text="Cancel"}}
```

**`select>option[value="a"]+option[value="b"]`**

```
{{select}}
  {{option value="a"}}
  {{option value="b"}}
{{/select}}
```

### Climb-up `^`

Moves context up one level per caret. The element following `^` becomes a sibling of the parent, not the current element. Multiple carets climb multiple levels.

**`div>input^button{Save}`**

```
{{div}}
  {{input}}
{{/div}}
{{button text="Save"}}
```

**`div>note>input^^button{Save}`**

```
{{div}}
  {{note}}
    {{input}}
  {{/note}}
{{/div}}
{{button text="Save"}}
```

### Multiplication `*`

Repeats an element a given number of times.

**`option*3`**

```
{{option}}
{{option}}
{{option}}
```

**`select>option*3`**

```
{{select}}
  {{option}}
  {{option}}
  {{option}}
{{/select}}
```

### Grouping `()`

Groups a sub-expression so that multiplication or combinators apply to the whole group.

**`(div>input)*2`**

```
{{div}}
  {{input}}
{{/div}}
{{div}}
  {{input}}
{{/div}}
```

**`div>(h1{Title}+span{Content})+button{Save}`**

```
{{div}}
  # Title
  Content
  {{button text="Save"}}
{{/div}}
```

---

## Attribute Operators

### ID `#`

Sets the `id` attribute using CSS selector notation.

**`input#username`**

```
{{input id="username"}}
```

**`button#submit{Submit}`**

```
{{button id="submit" text="Submit"}}
```

### Class `.`

Sets the `class` attribute. Multiple classes can be chained.

**`textarea.commit`**

```
{{textarea class="commit"}}
{{/textarea}}
```

**`div.panel.copy`**

```
{{div class="panel copy"}}
{{/div}}
```

Some tags treat certain class names as **semantic aliases** — see [Class aliases](#class-aliases).

### Custom attributes `[...]`

Attributes are written inside square brackets. Multiple attributes can be listed in a single bracket group (space-separated) or in separate groups. Values can be double-quoted, single-quoted, or unquoted (when the value contains no spaces).

**`input[type="text" placeholder="Full name"]`**

```
{{input type="text" placeholder="Full name"}}
```

**`input[type='text' placeholder='Full name']`**

```
{{input type="text" placeholder="Full name"}}
```

**`input[type=text placeholder=Name]`**

```
{{input type="text" placeholder="Name"}}
```

**`input[type="text"][placeholder="Name"]`**

```
{{input type="text" placeholder="Name"}}
```

**`input[placeholder="https://example.com"]`**

```
{{input placeholder="https://example.com"}}
```

> Output is always normalized to double-quoted values regardless of the input quoting style.

Paired tags include a closing tag:

**`checkbox-group[column]`**

```
{{checkbox-group column}}
{{/checkbox-group}}
```

### Item numbering `$`

The `$` placeholder is replaced by the iteration index (starting at 1) when used with multiplication. Multiple `$` signs pad with leading zeroes. Multiple `$` placeholders in the same string are all substituted independently.

**`option[value="item$"]*3`**

```
{{option value="item1"}}
{{option value="item2"}}
{{option value="item3"}}
```

**`option[value="item$$$"]*3`**

```
{{option value="item001"}}
{{option value="item002"}}
{{option value="item003"}}
```

**`input[id="field$"]*3`**

```
{{input id="field1"}}
{{input id="field2"}}
{{input id="field3"}}
```

**`input[id="f$" name="f$"]*2`**

```
{{input id="f1" name="f1"}}
{{input id="f2" name="f2"}}
```

`$` works in any position — attribute values, class names, and text content.

#### Reverse and custom-start numbering

Append `@-` to reverse the count, or `@N` to start from a custom base. The modifiers follow immediately after the `$` group.

**`option[value=$@-]*3`**

```
{{option value="3"}}
{{option value="2"}}
{{option value="1"}}
```

**`option[value=$@3]*3`**

```
{{option value="3"}}
{{option value="4"}}
{{option value="5"}}
```

---

## Text Content `{}`

Curly braces set the text content of an element.

For **directive** elements, text becomes the `text` attribute. For **CommonMark** elements (`h1`–`h6`, `span`, `li`), text is rendered inline.

**`button{Save}`**

```
{{button text="Save"}}
```

**`button{Submit}[color="success"]`**

```
{{button text="Submit" color="success"}}
```

**`h1{Introduction}`**

```
# Introduction
```

**`span{Hello world}`**

```
Hello world
```

**`h2{Section $}*3`**

```
## Section 1
## Section 2
## Section 3
```

---

## CommonMark Output

Some elements expand to plain Markdown rather than directive syntax, as long as they carry no classes or custom attributes.

### Headings `h1`–`h6`

**`h1{Title}`**

```
# Title
```

**`h2{Subtitle}`**

```
## Subtitle
```

**`h3{Section}`**

```
### Section
```

Adding a class or attribute switches to directive syntax:

**`h1.commit{Title}`**

```
{{h1 class="commit" text="Title"}}
{{/h1}}
```

**`h2.copy{Subtitle}`**

```
{{h2 class="copy" text="Subtitle"}}
{{/h2}}
```

### Spans

`span` without modifiers outputs its text inline. Adding a class or attribute switches to directive syntax.

**`span{Some text}`**

```
Some text
```

**`span.commit{Hello}`**

```
{{span class="commit" text="Hello"}}
{{/span}}
```

### Lists

`ul` and `ol` generate Markdown list syntax. The parent element is consumed — only the list items are emitted.

**`ul>li*3`**

```
-
-
-
```

**`ol>li*3`**

```
1.
2.
3.
```

**`ul>li{Item $}*3`**

```
- Item 1
- Item 2
- Item 3
```

**`ol>li{Step $}*3`**

```
1. Step 1
2. Step 2
3. Step 3
```

---

## HAI-specific Behaviour

### Class aliases

For the `note` element, the class names `info`, `tip`, `success`, `warning`, and `error` are recognized and included in `class`.

**`note.info`**

```
{{note class="info"}}
{{/note}}
```

**`note.warning`**

```
{{note class="warning"}}
{{/note}}
```

**`note.warning.fold`**

```
{{note class="warning fold"}}
{{/note}}
```

**`note.info{Be careful}`**

```
{{note class="info" text="Be careful"}}
{{/note}}
```

### Attribute ordering

Attributes are always emitted in this order, regardless of how they appear in the abbreviation:

1. `id` (from `#`)
2. `class` (from `.`)
3. `text` (from `{}`)
4. Explicit attributes (from `[...]`, in declaration order)

---

## Notes

**Space terminates an abbreviation**, except inside `{text}` braces and `[attribute]` brackets, where spaces are part of the value. The Tab key expands the token immediately before the cursor. Keep abbreviations compact; there is no need for them to be human-readable before expansion.

**Output is normalized.** Regardless of how attribute values are quoted in the abbreviation (double quotes, single quotes, or unquoted), the expanded output always uses double quotes.