# Macro Utility Library

## `hai.doc`

- `getContent(): string`
- `getDirectiveAttValue(dir: string, att: string): string | undefined`
- `getDirectiveByAttVal(att: string, val: string, dir?): string | undefined`
- `getDirectiveById(id: string, dir?: string): string | undefined`
- `getDirectivesByAttVal(att: string, val: string, dir?: string): string[]`
- `getDirectivesByClass(class: string, dir?: string): string[]`
- `insertDirectiveAfter(dir: string, targetDir: string): void`
- `insertDirectiveBefore(dir: string, targetDir: string): void`
- `removeDirective(dir: string): void`
- `removeDirectives(dirs: string[]): void`
- `replaceDirective(dir: string, replacement: string): void`
- `setContent(content: string): void`
- `setDirectiveAttValue(dir: string, att: string, val: string): void`
