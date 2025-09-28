# Macro Utility Library

## `hai.doc`

- `getContent(): string`
- `getDirective(att: string, val: string, dir?): string | undefined`
- `getDirectiveAttValue(dir: string, att: string): string | undefined`
- `getDirectiveById(id: string, dir?: string): string | undefined`
- `getDirectives(att: string, val: string, dir?: string): string[]`
- `getDirectivesByClass(class: string, dir?: string): string[]`
- `insertDirectiveAfter(dir: string, refDir: string): void`
- `insertDirectiveBefore(dir: string, refDir: string): void`
- `removeDirective(dir: string): void`
- `removeDirectives(dirs: string[]): void`
- `replaceDirective(dir: string, refDir: string): void`
- `setContent(content: string): void`
- `setDirectiveAttValue(dir: string, att: string, val: string): void`
