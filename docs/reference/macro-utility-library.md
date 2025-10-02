# Macro Utility Library

The global `hai` library exposes `app` and `doc` utility functions.

```
hai = {
  app,
  doc
}
```

:::tip
Use object destructuring:
```javascript
const {app, doc} = hai;

app.notify({
    type: "info", 
    message: "JS destructing for the win!",
    sticky: true
});

doc.setContent("I'm a JS ninja");
```
:::

## hai.app

### notify

```
notify(notification: {
    message: string
    type?: "info" | "success" | "warning" | "error"
    sticky?: boolean
    title?: string
})
```

## hai.doc

A set of handy functions to work with the document and its directives.

### getContent
```
getContent(): string
```

### getDirectiveAtt
```
getDirectiveAttValue(dir: string, att: string): {name: string; value?: string} | undefined
```

### getDirectiveAttValue
```
getDirectiveAttValue(dir: string, att: string): string | undefined
```

### getDirectiveByAttVal
```
getDirectiveByAttVal(att: string, val: string, dir?): string | undefined
```

### getDirectiveById
```
getDirectiveById(id: string, dir?: string): string | undefined
```

### getDirectivesByAttVal
```
getDirectivesByAttVal(att: string, val: string, dir?: string): string[]
```

### getDirectivesByClass
```
getDirectivesByClass(class: string, dir?: string): string[]
```

### getTitle
```
getTitle(): string | undefined
```

### insertDirectiveAfter
```
insertDirectiveAfter(dir: string, targetDir: string): void
```

### insertDirectiveBefore
```
insertDirectiveBefore(dir: string, targetDir: string): void
```

### removeDirective
```
removeDirective(dir: string): void
```

### removeDirectiveAtt
```
removeDirectiveAtt(dir: string, att: string): void
```

### removeDirectives
```
removeDirectives(dirs: string[]): void
```

### replaceDirective
```
replaceDirective(dir: string, replacement: string): void
```

### setContent
```
setContent(content: string): void
```

### setDirectiveAttValue
```
setDirectiveAttValue(dir: string, att: string, val: string): void
```

### setTitle
```
setTitle(title: string): void
```