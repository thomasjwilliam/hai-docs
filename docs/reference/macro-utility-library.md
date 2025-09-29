# Macro Utility Library

## `hai.doc`

- `getContent(): string`
- `getDirAttValue(dir: string, att: string): string | undefined`
- `getDirByAttVal(att: string, val: string, dir?): string | undefined`
- `getDirById(id: string, dir?: string): string | undefined`
- `getDirs(att: string, val: string, dir?: string): string[]`
- `getDirsByClass(class: string, dir?: string): string[]`
- `insertDirAfter(dir: string, targetDir: string): void`
- `insertDirBefore(dir: string, targetDir: string): void`
- `removeDir(dir: string): void`
- `removeDirs(dirs: string[]): void`
- `replaceDir(dir: string, replacement: string): void`
- `setContent(content: string): void`
- `setDirAttValue(dir: string, att: string, val: string): void`
