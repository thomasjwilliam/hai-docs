# Markdown Directives

## About

Markdown directives are custom syntax elements that enable you to build interactive pages directly in your documents. They provide form inputs, buttons, containers, and other UI components that can be used to create dynamic templates and workflows.

**Syntax:** Directives use double curly braces `{{` and `}}` with a tag name and optional attributes:

```
{{tagName attribute="value" booleanAttribute}}
```

**Self-closing vs Container directives:**
- Self-closing: `{{input type="text"}}`
- Container: `{{note}}content here{{/note}}`

---

## Global Attributes

These attributes are available on all directives:

### id

A unique identifier for the directive within the document. Useful for targeting specific elements in macros using the `hai.doc` utility library.

**Example:**
```
{{input id="customer-name" type="text"}}
```

### class

Space-separated CSS class names for styling or grouping directives. Can be used to target multiple elements in macros.

**Example:**
```
{{div class="highlight important"}}
  Important content
{{/div}}
```

### aria-label

Accessibility label for screen readers and assistive technologies.

**Example:**
```
{{button aria-label="Submit form" onclick="$SubmitMacro" text="Submit"}}
```

---

## Form Directives

Form directives provide interactive input elements for collecting and displaying data.

:::tip

**Save directives as snippets**

Instead of typing directives repeatedly, save them as snippets. For example, create a snippet called `DatePicker`:
```
{{input type="date"}}
```

Then insert it with `/DatePicker` or reference it in other snippets as `$DatePicker`.

:::

### Button

Triggers a macro or snippet when clicked. Similar to the HTML [button](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/button) element.

**Syntax:**
```
{{button ...attributes}}
```

**Attributes:**

`onclick` **required**  
The trigger for a snippet or macro, prefixed with `$`. Use the format `$SnippetName` or `$SnippetName[ordinal]` if multiple snippets share the same trigger.

`text` **required**  
The text displayed on the button.

`color`  
Button color theme. Default: `primary`  
Values: `primary` | `secondary` | `accent` | `info` | `success` | `warning` | `error`

`aria-label`  
Accessibility label (optional, defaults to button text)

`id`, `class`  
Standard global attributes

**Examples:**

```
{{button onclick="$SaveData" text="Save"}}
```

```
{{button onclick="$DeleteItem" text="Delete" color="error"}}
```

```
{{button onclick="$ProcessOrder[2]" text="Process" color="success"}}
```

**Usage with Snippets:**
```
First Name: {{input id="first-name"}}
Last Name: {{input id="last-name"}}
{{button onclick="$CreateProfile" text="Create Profile"}}
```

### Checkbox Group

Renders multiple checkboxes that can be individually toggled. Similar to HTML [checkbox](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/checkbox) elements.

**Syntax:**
```
{{checkbox-group ...attributes}}
  {{option value="..."}}
  {{option value="..." label="..."}}
{{/checkbox-group}}
```

**Attributes:**

`column`  
Boolean attribute. When present, displays checkboxes vertically instead of horizontally.

`value`  
Comma-separated list of checked checkbox values. Commas within values are automatically escaped.

`aria-label`, `id`, `class`  
Standard global attributes

**Examples:**

```
{{checkbox-group}}
  {{option value="email"}}
  {{option value="sms" label="Text Messages"}}
  {{option value="phone" label="Phone Calls"}}
{{/checkbox-group}}
```

```
{{checkbox-group value="email,sms" column}}
  {{option value="email" label="Email Notifications"}}
  {{option value="sms" label="SMS Notifications"}}
  {{option value="phone" label="Phone Notifications"}}
{{/checkbox-group}}
```

**Navigation:** Use Tab/Shift+Tab to navigate between checkboxes within the group and to other form elements.

### Datalist

Provides a searchable dropdown with the ability to enter custom text. Similar to HTML [datalist](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/datalist) element.

**Syntax:**
```
{{datalist ...attributes}}
  {{option value="..."}}
  {{option value="..." label="..."}}
{{/datalist}}
```

**Attributes:**

`placeholder`  
Text displayed when no value is selected.

`value`  
The selected option value or custom entered text.

`aria-label`, `id`, `class`  
Standard global attributes

**Examples:**

```
{{datalist placeholder="Select your role"}}
  {{option value="Developer"}}
  {{option value="Designer"}}
  {{option value="Manager"}}
  {{option value="Other"}}
{{/datalist}}
```

```
{{datalist placeholder="Choose a country" value="France"}}
  {{option value="France"}}
  {{option value="Germany"}}
  {{option value="Italy"}}
  {{option value="Spain"}}
{{/datalist}}
```

**Keyboard Shortcuts:**
- Arrow keys: Navigate options
- Enter: Select highlighted option
- Escape: Close dropdown
- Tab/Shift+Tab: Navigate to next/previous form element
- Cmd/Ctrl + Enter: Commit value and exit field
- Cmd/Ctrl + P: Commit value (with quick input mode enabled, automatically moves to next field)

### Input

Text input field for various data types. Similar to HTML [input](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input) element.

**Syntax:**
```
{{input ...attributes}}
```

**Attributes:**

`type`  
Input type. Default: `text`  
Values: `text` | `date` | `datetime-local`

`placeholder`  
Text displayed when the field is empty.

`value`  
Current value of the input.

`cols`  
For `type="text"`: Sets the visible width.
- Positive integer: Minimum width in characters (can grow with content)
- `auto`: Full width (100% of container)
- Default: `12` characters

`commit`  
Boolean attribute. When present, the input value instantly replaces the input field after entry. Useful for quick text insertion.

`aria-label`, `id`, `class`  
Standard global attributes

**Examples:**

**Basic text input:**
```
{{input}}
```

**Text with placeholder:**
```
{{input type="text" placeholder="Enter your name"}}
```

**Full-width input:**
```
{{input type="text" cols="auto" placeholder="Full width input"}}
```

**Date picker:**
```
{{input type="date" value="2024-01-15"}}
```

**Date and time picker:**
```
{{input type="datetime-local" value="2024-01-15T14:30"}}
```

**Quick commit input:**
```
{{input type="text" placeholder="Quick entry..." commit}}
```

**Keyboard Shortcuts:**
- Tab/Shift+Tab: Navigate to next/previous form element
- Cmd/Ctrl + Enter: Exit field (cursor moves after the input)
- Cmd/Ctrl + P: Commit value (replaces input with text)
- Cmd/Ctrl + Backspace/Delete: Remove the input directive

### Option

Defines an option for `checkbox-group`, `datalist`, `radio-group`, or `select` directives. Similar to HTML [option](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/option) element.

**Syntax:**
```
{{option ...attributes}}
```

**Attributes:**

`value` **required**  
The value of the option.

`label`  
Display label for the option. If not provided, the value is used as the label.

`aria-label`, `id`, `class`  
Standard global attributes

**Examples:**

```
{{option value="small"}}
```

```
{{option value="md" label="Medium"}}
```

```
{{option value="xl" label="Extra Large"}}
```

### Radio Group

Allows selection of a single option from a group. Similar to HTML [input type="radio"](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/radio) element.

**Syntax:**
```
{{radio-group ...attributes}}
  {{option value="..."}}
  {{option value="..." label="..."}}
{{/radio-group}}
```

**Attributes:**

`column`  
Boolean attribute. When present, displays radio buttons vertically instead of horizontally.

`value`  
The value of the selected radio option.

`aria-label`, `id`, `class`  
Standard global attributes

**Examples:**

```
{{radio-group}}
  {{option value="yes"}}
  {{option value="no"}}
  {{option value="maybe"}}
{{/radio-group}}
```

```
{{radio-group value="high" column}}
  {{option value="low" label="Low Priority"}}
  {{option value="medium" label="Medium Priority"}}
  {{option value="high" label="High Priority"}}
{{/radio-group}}
```

**Navigation:** Use Tab/Shift+Tab to navigate between radio buttons within the group and to other form elements.

### Select

Dropdown menu for selecting a single option. Similar to HTML [select](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/select) element.

**Syntax:**
```
{{select ...attributes}}
  {{option value="..."}}
  {{option value="..." label="..."}}
{{/select}}
```

**Attributes:**

`placeholder`  
Text displayed when no option is selected.

`value`  
The value of the selected option.

`aria-label`, `id`, `class`  
Standard global attributes

**Examples:**

```
{{select placeholder="Choose size"}}
  {{option value="S" label="Small"}}
  {{option value="M" label="Medium"}}
  {{option value="L" label="Large"}}
  {{option value="XL" label="Extra Large"}}
{{/select}}
```

```
{{select placeholder="Priority Level" value="Medium"}}
  {{option value="Low"}}
  {{option value="Medium"}}
  {{option value="High"}}
  {{option value="Critical"}}
{{/select}}
```

**Keyboard Shortcuts:**
- Arrow keys: Navigate options
- Enter/Space: Select option
- Escape: Close dropdown
- Tab/Shift+Tab: Navigate to next/previous form element
- Cmd/Ctrl + Enter: Exit field
- Cmd/Ctrl + P: Commit selected value

### Textarea

Multi-line text input field. Similar to HTML [textarea](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/textarea) element.

**Syntax:**
```
{{textarea ...attributes}}
```

**Attributes:**

`placeholder`  
Text displayed when the field is empty.

`value`  
Current text content. Multi-line text uses `\n` for line breaks in the attribute.

`rows`  
Visible height in lines. Default: `auto` (auto-resizes to content)  
Values: Positive integer or `auto`

`cols`  
Visible width in characters.
- Positive integer: Fixed width
- `auto`: Full width (100% of container)
- Default: 3 columns

`commit`  
Boolean attribute. When present, the entered text instantly replaces the textarea after entry.

`aria-label`, `id`, `class`  
Standard global attributes

**Examples:**

```
{{textarea}}
```

```
{{textarea placeholder="Enter your message..."}}
```

```
{{textarea placeholder="Description" value="Default text here"}}
```

```
{{textarea placeholder="Comments" cols="60" rows="6"}}
```

```
{{textarea placeholder="Quick note" commit}}
```

**Keyboard Shortcuts:**
- Tab/Shift+Tab: Navigate to next/previous form element
- Cmd/Ctrl + Enter: Exit field
- Cmd/Ctrl + P: Commit value
- Cmd/Ctrl + Backspace/Delete: Remove the textarea

---

## Container Directives

Container directives wrap content and provide organizational and interactive features.

### Note

Displays content in a styled container with optional title and type-specific styling. Supports actions like folding, copying, and committing.

**Syntax:**
```
{{note ...attributes}}
  content here
{{/note}}
```

**Attributes:**

`type`  
Visual style of the note. Each type has a distinct color and icon:
- `info` - Blue, information icon
- `tip` - Green, lightbulb icon
- `success` - Green, checkmark icon
- `warning` - Orange, alert icon
- `error` - Red, error icon

`title`  
Custom title for the note. If not provided and `type` is set, uses the type name as the title.

`fold`  
Boolean attribute. Adds a button to collapse/expand the note content.

`commit`  
Boolean attribute. Adds a button to replace the note with its inner content only.

`copy`  
Boolean attribute. Adds a button to copy the note's content to clipboard (as HTML).

`remove`  
Boolean attribute. Adds a button to delete the note and its content.

`aria-label`, `id`, `class`  
Standard global attributes

**Examples:**

**Simple note:**
```
{{note}}
  This is a simple note without styling.
{{/note}}
```

**Info note with folding:**
```
{{note type="info" fold}}
  This information can be collapsed to save space.
{{/note}}
```

**Success note with title:**
```
{{note type="success" title="Task Completed"}}
  Your request has been processed successfully.
{{/note}}
```

**Error note with custom title:**
```
{{note type="error" title="Validation Failed"}}
  Please check the following errors:
  - Name is required
  - Email format is invalid
{{/note}}
```

**Tip with all actions:**
```
{{note type="tip" fold commit copy remove}}
  **Pro Tip:** Use keyboard shortcuts to work faster!
  
  Press `Cmd/Ctrl + P` to commit form values.
{{/note}}
```

**Nested content:**
```
{{note type="info" title="Order Summary"}}
  Customer: {{input placeholder="Name"}}
  Item: {{select placeholder="Product"}}
    {{option value="widget"}}
    {{option value="gadget"}}
  {{/select}}
{{/note}}
```

### Div

Generic block container for grouping content with optional visual styling and actions.

**Syntax:**
```
{{div ...attributes}}
  content here
{{/div}}
```

**Attributes:**

`fold`  
Boolean attribute. Adds a button to collapse/expand the div content.

`commit`  
Boolean attribute. Adds a button to replace the div with its inner content only.

`copy`  
Boolean attribute. Adds a button to copy the div's content to clipboard (as HTML).

`remove`  
Boolean attribute. Adds a button to delete the div and its content.

`shaded`  
Boolean attribute. Applies a subtle gray background color for visual separation.

`aria-label`, `id`, `class`  
Standard global attributes

**Examples:**

**Simple container:**
```
{{div}}
  Grouped content here
{{/div}}
```

**Shaded section:**
```
{{div shaded}}
  This section has a light gray background for emphasis.
{{/div}}
```

**Collapsible section with all actions:**
```
{{div fold commit copy remove}}
  Important content that can be:
  - Folded to save space
  - Committed (unwrapped)
  - Copied to clipboard
  - Removed entirely
{{/div}}
```

**Nested structure:**
```
{{div class="form-section" shaded}}
  ## Personal Information
  
  Name: {{input type="text"}}
  Email: {{input type="text"}}
  
  {{div class="subsection"}}
    Date of Birth: {{input type="date"}}
  {{/div}}
{{/div}}
```

### Span

Inline container for wrapping text or inline elements with optional actions. Useful for marking up specific portions of text.

**Syntax:**
```
{{span ...attributes}}
  inline content here
{{/span}}
```

**Attributes:**

`commit`  
Boolean attribute. Adds a button to replace the span with its inner content only.

`copy`  
Boolean attribute. Adds a button to copy the span's content to clipboard (as HTML).

`remove`  
Boolean attribute. Adds a button to delete the span and its content.

`aria-label`, `id`, `class`  
Standard global attributes

**Examples:**

**Highlight text:**
```
This is {{span class="highlight"}}important text{{/span}} in a sentence.
```

**Inline form elements:**
```
Customer {{span}}{{input placeholder="Name"}}{{/span}} ordered {{span}}{{input placeholder="Quantity"}}{{/span}} items.
```

**With commit action:**
```
The answer is {{span commit}}{{input placeholder="42"}}{{/span}} according to the book.
```

---

## Advanced Features

### Commit Action

The "commit" action replaces a directive (and its nested directives) with their final values or content. This is useful for:
- Converting form templates into finalized text
- Removing directive markup while keeping the content
- Creating templates that transform into plain text

**How it works:**
1. For input directives: Replaces with the entered value
2. For container directives (note, div, span): Replaces with inner content only
3. For button directives: Removes the button entirely
4. Processes nested directives recursively from innermost to outermost

**Keyboard Shortcut:** `Cmd/Ctrl + Alt + P` commits all committable directives in the document

**Example workflow:**
```
Before commit:
{{note type="info" commit}}
  Customer: {{input value="John Doe"}}
  Order: {{input value="Widget"}}
{{/note}}

After commit:
Customer: John Doe
Order: Widget
```

### Copy to Clipboard

The "copy" action copies the content of a directive to the clipboard as HTML. The content is processed (committed) before copying, so form values are included.

**Use cases:**
- Copying finalized form content to paste elsewhere
- Extracting sections for use in other documents
- Creating reusable content blocks

**Action buttons:** Look for the copy icon (📋) in the directive's button bar.

### Fold/Unfold

Directives with the `fold` attribute can collapse and expand their content to save screen space.

**Behavior:**
- Collapsed: Shows only the header/title
- Expanded: Shows full content
- State is visual only (not persisted to markdown)

**Use cases:**
- Long notes or sections you want to hide temporarily
- Creating collapsible FAQ sections
- Managing complex forms with many fields

### Keyboard Navigation

Form directives support efficient keyboard navigation:

**General Navigation:**
- `Tab`: Move to next form element
- `Shift + Tab`: Move to previous form element

**Within Input Fields:**
- `Cmd/Ctrl + Enter`: Exit field and place cursor after the directive
- `Cmd/Ctrl + P`: Commit the value (replaces directive with text)
- `Cmd/Ctrl + Backspace/Delete`: Remove the directive

**Quick Input Mode:**
When enabled (`Cmd/Ctrl + Alt + Q`):
- Committing a value automatically jumps to the next form field
- Enables rapid form filling without manual navigation

**Within Groups (checkbox-group, radio-group):**
- `Tab`: Move to next checkbox/radio within group, or to next form element after the last option
- `Shift + Tab`: Move to previous checkbox/radio, or to previous form element before the first option

---

## Practical Examples

### Customer Information Form

```
# Customer Information

**Personal Details**
First Name: {{input id="first-name" placeholder="John"}}
Last Name: {{input id="last-name" placeholder="Doe"}}
Email: {{input type="text" placeholder="john@example.com"}}
Phone: {{input type="text" placeholder="+1 234 567 8900"}}

**Preferences**
Contact method:
{{checkbox-group column}}
  {{option value="email" label="Email"}}
  {{option value="phone" label="Phone"}}
  {{option value="sms" label="SMS"}}
{{/checkbox-group}}

Priority Level:
{{radio-group value="normal"}}
  {{option value="low" label="Low"}}
  {{option value="normal" label="Normal"}}
  {{option value="high" label="High"}}
{{/radio-group}}

{{button onclick="$ProcessCustomer" text="Submit" color="primary"}}
```

### Support Ticket Template

```
{{note type="info" title="Support Ticket" fold}}
  **Ticket Information**
  Customer: {{input placeholder="Customer name"}}
  Issue Type: {{select placeholder="Select issue type"}}
    {{option value="technical" label="Technical Problem"}}
    {{option value="billing" label="Billing Question"}}
    {{option value="feature" label="Feature Request"}}
  {{/select}}
  
  Priority: {{radio-group}}
    {{option value="low"}}
    {{option value="medium"}}
    {{option value="high"}}
  {{/radio-group}}
  
  **Description**
  {{textarea placeholder="Describe the issue..." rows="6"}}
  
  {{button onclick="$CreateTicket" text="Create Ticket" color="success"}}
{{/note}}
```

### Meeting Notes with Sections

```
# Meeting Notes - {{input type="date"}}

{{div shaded fold}}
  **Attendees**
  {{textarea placeholder="List attendees..." rows="3"}}
{{/div}}

{{div shaded fold}}
  **Discussion Points**
  {{textarea placeholder="Key points discussed..." rows="5"}}
{{/div}}

{{div shaded fold}}
  **Action Items**
  {{textarea placeholder="Action items and owners..." rows="5"}}
{{/div}}

{{div shaded fold}}
  **Next Meeting**
  Date: {{input type="date"}}
  Time: {{input placeholder="10:00 AM"}}
{{/div}}
```

### Quick Data Entry

Enable quick input mode (`Cmd/Ctrl + Alt + Q`) and use commit attributes for rapid data entry:

```
Order Entry:
Product: {{input placeholder="Product name" commit}}
Quantity: {{input placeholder="0" commit}}
Price: {{input placeholder="$0.00" commit}}
Customer: {{datalist placeholder="Select customer" commit}}
  {{option value="Acme Corp"}}
  {{option value="Global Industries"}}
  {{option value="Tech Solutions"}}
{{/datalist}}

{{button onclick="$ProcessOrder" text="Process Order"}}
```

Press `Cmd/Ctrl + P` on each field to commit and auto-advance to the next field.

### Nested Directives

```
{{note type="tip" title="Project Setup" fold commit}}
  ## Configuration
  
  {{div class="config-section"}}
    Project Name: {{input value="My Project"}}
    
    Environment:
    {{select value="dev"}}
      {{option value="dev" label="Development"}}
      {{option value="staging" label="Staging"}}
      {{option value="prod" label="Production"}}
    {{/select}}
    
    {{div class="features"}}
      Enable Features:
      {{checkbox-group value="api,auth"}}
        {{option value="api" label="REST API"}}
        {{option value="auth" label="Authentication"}}
        {{option value="db" label="Database"}}
      {{/checkbox-group}}
    {{/div}}
  {{/div}}
  
  {{button onclick="$GenerateConfig" text="Generate Config" color="primary"}}
{{/note}}
```

---

## Tips & Best Practices

1. **Use snippets for common patterns:** Save frequently-used directive combinations as snippets for quick insertion.

2. **Enable quick input mode:** Use `Cmd/Ctrl + Alt + Q` to toggle quick input mode for faster form filling.

3. **Use meaningful IDs:** Assign descriptive `id` attributes to make directives easier to target in macros.

4. **Group related inputs:** Use `div` or `note` to organize related form fields logically.

5. **Commit for finalization:** Use `commit` attributes or actions to convert templates into final text output.

6. **Fold long sections:** Add `fold` to lengthy notes or divs to improve document readability.

7. **Use appropriate types:** Choose the right input `type` (text, date, datetime-local) for your data.

8. **Leverage Tab navigation:** Use Tab/Shift+Tab to navigate efficiently between form elements.

9. **Use placeholder text:** Provide clear placeholder text to guide users on expected input.

10. **Combine with macros:** Use buttons with `onclick` to trigger macros that process directive values.
