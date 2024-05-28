# Snippet

## Snippet

A snippet is a piece of text which allows you to quickly insert frequently used phrases, templates, or boilerplate text,
reduce repetitive typing and save time.

A snippet is defined by:

- **title**: a short description, used to explain the concept of the snippet
- **trigger**: a unique, case-insensitive word used to identify the snippet and "trigger" it while typing or referencing
  it in other snippets
- **note**: optional content in plain or markdown-formatted text to provide extra information about the snippet
- **text**: the actual snippet, written in plain or markdown-formatted text with twists and can be injected into
  your writing by selecting it from the library. A snippet has access to
  other snippets and macros, allowing you to compose an entire document automatically from different individual snippets
  of text.

A snippet can have multiple texts, published or drafted, and reordered with drag-and-drop.

## Macro

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

A macro can have multiple scripts, published or drafted, and reordered with drag-and-drop.
