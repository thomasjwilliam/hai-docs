# Emmet Abbreviation Syntax

HAI supports a subset of [Emmet](https://emmet.io) abbreviation syntax in the Markdown editor. Type an abbreviation and press **Tab** to expand it into a HAI directive or CommonMark markup.

Output is not HTML — it is either a **HAI directive** (`{{tag ...}}` / `{{/tag}}`) or **CommonMark markdown** (headings, lists, plain text), depending on the element.

---

## Elements

Abbreviations start with a tag name. Only recognised HAI directive tags are valid:

`div`, `note`, `span`, `input`, `textarea`, `button`, `select`, `datalist`, `option`, `checkbox-group`, `radio-group`, `p`, `h1`–`h6`, `ul`, `ol`, `li`

| Abbreviation | Expands to                         |
|--------------|------------------------------------|
| `div`        | `{{div}}`<br/>`{{/div}}`           |
| `input`      | `{{input}}`                        |
| `textarea`   | `{{textarea}}`<br/>`{{/textarea}}` |

If the abbreviation starts with `.`, `#`, or `[` (no explicit tag), the tag defaults to `div`.

| Abbreviation          | Expands to                                 |
|-----------------------|--------------------------------------------|
| `.panel`              | `{{div class="panel"}}`<br/>`{{/div}}`     |
| `#main`               | `{{div id="main"}}`<br/>`{{/div}}`         |
| `[aria-label="main"]` | `{{div aria-label="main"}}`<br/>`{{/div}}` |

---

## Nesting Operators

### Child `>`

Nests elements inside each other.

| Abbreviation        | Expands to                                                                           |
|---------------------|--------------------------------------------------------------------------------------|
| `select>option`     | `{{select}}`<br/>`  {{option}}`<br/>`{{/select}}`                                    |
| `div>select>option` | `{{div}}`<br/>`  {{select}}`<br/>`    {{option}}`<br/>`  {{/select}}`<br/>`{{/div}}` |

### Sibling `+`

Places elements at the same level.

| Abbreviation                                 | Expands to                                                                               |
|----------------------------------------------|------------------------------------------------------------------------------------------|
| `button{Save}+button{Cancel}`                | `{{button text="Save"}}`<br/>`{{button text="Cancel"}}`                                  |
| `select>option[value="a"]+option[value="b"]` | `{{select}}`<br/>`  {{option value="a"}}`<br/>`  {{option value="b"}}`<br/>`{{/select}}` |

### Climb-up `^`

Moves context up one level per caret. The element following `^` becomes a sibling of the parent, not the current element. Multiple carets climb multiple levels.

| Abbreviation                   | Expands to                                                                                                   |
|--------------------------------|--------------------------------------------------------------------------------------------------------------|
| `div>input^button{Save}`       | `{{div}}`<br/>`  {{input}}`<br/>`{{/div}}`<br/>`{{button text="Save"}}`                                      |
| `div>note>input^^button{Save}` | `{{div}}`<br/>`  {{note}}`<br/>`    {{input}}`<br/>`  {{/note}}`<br/>`{{/div}}`<br/>`{{button text="Save"}}` |

### Multiplication `*`

Repeats an element a given number of times.

| Abbreviation      | Expands to                                                                              |
|-------------------|-----------------------------------------------------------------------------------------|
| `option*3`        | `{{option}}`<br/>`{{option}}`<br/>`{{option}}`                                          |
| `select>option*3` | `{{select}}`<br/>`  {{option}}`<br/>`  {{option}}`<br/>`  {{option}}`<br/>`{{/select}}` |

### Grouping `()`

Groups a sub-expression so that multiplication or combinators apply to the whole group.

| Abbreviation                                 | Expands to                                                                                |
|----------------------------------------------|-------------------------------------------------------------------------------------------|
| `(div>input)*2`                              | `{{div}}`<br/>`  {{input}}`<br/>`{{/div}}`<br/>`{{div}}`<br/>`  {{input}}`<br/>`{{/div}}` |
| `div>(h1{Title}+span{Content})+button{Save}` | `{{div}}`<br/>`  # Title`<br/>`  Content`<br/>`  {{button text="Save"}}`<br/>`{{/div}}`   |

---

## Attribute Operators

### ID `#`

Sets the `id` attribute using CSS selector notation.

| Abbreviation            | Expands to                             |
|-------------------------|----------------------------------------|
| `input#username`        | `{{input id="username"}}`              |
| `button#submit{Submit}` | `{{button id="submit" text="Submit"}}` |

### Class `.`

Sets the `class` attribute. Multiple classes can be chained.

| Abbreviation      | Expands to                                        |
|-------------------|---------------------------------------------------|
| `textarea.commit` | `{{textarea class="commit"}}`<br/>`{{/textarea}}` |
| `div.panel.copy`  | `{{div class="panel copy"}}`<br/>`{{/div}}`       |

Some tags treat certain class names as **semantic aliases** — see [Class aliases](#class-aliases).

### Custom attributes `[...]`

Attributes are written inside square brackets. Multiple attributes can be listed in a single bracket group (space-separated) or in separate groups. Values can be double-quoted, single-quoted, or unquoted (when the value contains no spaces).

| Abbreviation                                 | Expands to                                            |
|----------------------------------------------|-------------------------------------------------------|
| `input[type="text" placeholder="Full name"]` | `{{input type="text" placeholder="Full name"}}`       |
| `input[type='text' placeholder='Full name']` | `{{input type="text" placeholder="Full name"}}`       |
| `input[type=text placeholder=Name]`          | `{{input type="text" placeholder="Name"}}`            |
| `input[type="text"][placeholder="Name"]`     | `{{input type="text" placeholder="Name"}}`            |
| `checkbox-group[column]`                     | `{{checkbox-group column}}`<br/>`{{/checkbox-group}}` |
| `input[placeholder="https://example.com"]`   | `{{input placeholder="https://example.com"}}`         |

> Output is always normalized to double-quoted values regardless of the input quoting style.

### Item numbering `$`

The `$` placeholder is replaced by the iteration index (starting at 1) when used with multiplication. Multiple `$` signs pad with leading zeroes. Multiple `$` placeholders in the same string are all substituted independently.

| Abbreviation                 | Expands to                                                                                     |
|------------------------------|------------------------------------------------------------------------------------------------|
| `option[value="item$"]*3`    | `{{option value="item1"}}`<br/>`{{option value="item2"}}`<br/>`{{option value="item3"}}`       |
| `option[value="item$$$"]*3`  | `{{option value="item001"}}`<br/>`{{option value="item002"}}`<br/>`{{option value="item003"}}` |
| `input[id="field$"]*3`       | `{{input id="field1"}}`<br/>`{{input id="field2"}}`<br/>`{{input id="field3"}}`                |
| `input[id="f$" name="f$"]*2` | `{{input id="f1" name="f1"}}`<br/>`{{input id="f2" name="f2"}}`                                |

`$` works in any position — attribute values, class names, and text content.

#### Reverse and custom-start numbering

Append `@-` to reverse the count, or `@N` to start from a custom base. The modifiers follow immediately after the `$` group.

| Abbreviation          | Expands to                                                                   |
|-----------------------|------------------------------------------------------------------------------|
| `option[value=$@-]*3` | `{{option value="3"}}`<br/>`{{option value="2"}}`<br/>`{{option value="1"}}` |
| `option[value=$@3]*3` | `{{option value="3"}}`<br/>`{{option value="4"}}`<br/>`{{option value="5"}}` |

---

## Text Content `{}`

Curly braces set the text content of an element.

For **directive** elements, text becomes the `text` attribute. For **CommonMark** elements (`h1`–`h6`, `span`, `li`), text is rendered inline.

| Abbreviation                      | Expands to                                           |
|-----------------------------------|------------------------------------------------------|
| `button{Save}`                    | `{{button text="Save"}}`                             |
| `button{Submit}[color="success"]` | `{{button text="Submit" color="success"}}`           |
| `h1{Introduction}`                | `# Introduction`                                     |
| `span{Hello world}`               | `Hello world`                                        |
| `h2{Section $}*3`                 | `## Section 1`<br/>`## Section 2`<br/>`## Section 3` |

---

## CommonMark Output

Some elements expand to plain Markdown rather than directive syntax, as long as they carry no classes or custom attributes.

### Headings `h1`–`h6`

| Abbreviation   | Expands to    |
|----------------|---------------|
| `h1{Title}`    | `# Title`     |
| `h2{Subtitle}` | `## Subtitle` |
| `h3{Section}`  | `### Section` |

Adding a class or attribute switches to directive syntax:

| Abbreviation        | Expands to                                          |
|---------------------|-----------------------------------------------------|
| `h1.commit{Title}`  | `{{h1 class="commit" text="Title"}}`<br/>`{{/h1}}`  |
| `h2.copy{Subtitle}` | `{{h2 class="copy" text="Subtitle"}}`<br/>`{{/h2}}` |

### Spans

`span` without modifiers outputs its text inline. Adding a class or attribute switches to directive syntax.

| Abbreviation         | Expands to                                             |
|----------------------|--------------------------------------------------------|
| `span{Some text}`    | `Some text`                                            |
| `span.commit{Hello}` | `{{span class="commit" text="Hello"}}`<br/>`{{/span}}` |

### Lists

`ul` and `ol` generate Markdown list syntax. The parent element is consumed — only the list items are emitted.

| Abbreviation      | Expands to                                  |
|-------------------|---------------------------------------------|
| `ul>li*3`         | `- `<br/>`- `<br/>`- `                      |
| `ol>li*3`         | `1. `<br/>`2. `<br/>`3. `                   |
| `ul>li{Item $}*3` | `- Item 1`<br/>`- Item 2`<br/>`- Item 3`    |
| `ol>li{Step $}*3` | `1. Step 1`<br/>`2. Step 2`<br/>`3. Step 3` |

---

## HAI-specific Behaviour

### Class aliases

For the `note` element, the class names `info`, `tip`, `success`, `warning`, and `error` are recognized and included in `class`.

| Abbreviation            | Expands to                                                |
|-------------------------|-----------------------------------------------------------|
| `note.info`             | `{{note class="info"}}`<br/>`{{/note}}`                   |
| `note.warning`          | `{{note class="warning"}}`<br/>`{{/note}}`                |
| `note.warning.fold`     | `{{note class="warning fold"}}`<br/>`{{/note}}`           |
| `note.info{Be careful}` | `{{note class="info" text="Be careful"}}`<br/>`{{/note}}` |

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
