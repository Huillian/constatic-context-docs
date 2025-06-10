## ES6 Modules

### Understand how ES6 Modules work

#### ES6 Modules
This base uses `"type": "module"` in `package.json`. It is important to remember to use the `.js` extension when importing files from relative paths (even if they are TypeScript files).

If you export a function from a TypeScript file:

`src/functions/math/mycustumfunc.ts`
```typescript
export function sum(a: number, b: number){
    return a + b;
}
```
Import it by adding the `.js` extension at the end:

`src/functions/index.ts`
```typescript
import { sum } from "./math/mycustumfunc.js"
```
We can also use the `await` keyword at the top level of the code:

`hello.ts`
```typescript
import { setTimeout } from "node:timers/promises"

console.log("Hello");
await setTimeout(4000);
console.log("World");
```
If for some reason you need dynamic imports, you can use `import` as a function in the code:

`handler.ts`
```typescript
async function handle(path: string){
  const module = await import(path);
  // ...
}
```


