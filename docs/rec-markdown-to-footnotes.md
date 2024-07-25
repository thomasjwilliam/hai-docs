# Convert Markdown links to plain text footnotes

Markdown is a great way to format text and display clickable links. However, there are situations where rich text cannot be rendered, for example plain text email or plain text forms.

This recipe will show you how to convert Markdown links into footnotes in a single click.

## Instructions

Create the following Snippet written in Markdown:
```md title="About Markdown"
According to [Wikipedia](https://en.wikipedia.org/wiki/Snippet_(programming), a snippet is a programming term for a small region of re-usable [source code](https://en.wikipedia.org/wiki/Source_code), [machine code](https://en.wikipedia.org/wiki/Machine_code), or text.  
Ordinarily, these are formally defined operative units to incorporate into larger [programming modules](https://en.wikipedia.org/wiki/Module_(programming)).  
Snippet management is a feature of some [text editors](https://en.wikipedia.org/wiki/Text_editor), program [source code editors](https://en.wikipedia.org/wiki/Source_code_editor), [IDEs](https://en.wikipedia.org/wiki/Integrated_development_environment), and related [software](https://en.wikipedia.org/wiki/Software).   
It allows the user to avoid repetitive typing in the course of routine edit operations.
```

Create the following Macro:
```js title="Convert Markdown links to footnotes"
// hai on:trigger
// hai set:docContent

// Get the written text
let text = $hai.docContent;

// Regular expression to match markdown-style links
const mdLinkRE = /\[([^\]]+)\]\(([^)]+)\)/gm;

// Array to store footnotes
let footnotes = [];

// Index for footnotes (start at 1)
let idx = 1;

// Find all matches of markdown-style links
const matches = [...text.matchAll(mdLinkRE)];

// Iterate over each match
matches.forEach(([match, title, link]) => {
// Add footnote to the footnotes array
footnotes.push(`[${idx}] ${link}`);
// Replace the markdown-style link with the title and footnote index
text = text.replace(match, `${title} [${idx}]`);
// Increment the footnote index
idx++;
});

// If no footnotes were generated, return the original text
if (!footnotes.length) return text;

// Determine whitespace based on whether there are newlines at the end of the text
const whitespace = /\n\s*\n\s*$/.test(text) ? '' : '\r\n\r\n';

// Concatenate the original text, whitespace (if needed), and footnotes, and return
return text + whitespace + footnotes.join('\r\n');
```

Try it out:
1. Click the snippet to insert it in the composer
2. Click the macro to build the footnotes
