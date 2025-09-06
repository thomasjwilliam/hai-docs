# Macro

A macro is a piece of text written in JavaScript to create instructions which use and transform data. It _cannot_
reference and use other snippets or macros, but it is context-aware and has extra capabilities giving it some
powers to automate your work and environment.

A macro - just like a static snippet - is defined by:

- **title**: a short description, used to explain the concept of the macro
- **trigger**: a unique, case-insensitive word, used to identify the macro and "trigger" it while typing or referencing
  it in other snippets
- **note**: optional content in plain or markdown-formatted text to provide extra information about the macro
- **script**: the actual instructions written in JavaScript which are automatically wrapped in a function. The
  returned value can be used in other snippets or injected
  directly into your document.

A macro can have multiple scripts, published or drafted, and reordered.
