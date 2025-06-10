## Imports

### Import shortcuts for better code organization

#### Import Shortcuts
In this project, you will find the Node.js feature called import shortcuts. You can import anything using named shortcuts in `package.json`.

See how it looks in `package.json` and `tsconfig.json`.
With this, you can export everything from an `index` file of these shortcuts and easily import it anywhere in your code. See the example below:

Let's export this simple function from the functions folder:

`src/functions/math.ts`
```typescript
export function sum(a: number, b: number){
  return a + b;
}
```
Export it in the `index` file of the functions folder, which is defined in the `package.json` and `tsconfig.json` files:

`src/functions/index.ts`
```typescript
export * from "./math.js"
```
Note that since this project uses the `module` type, we need to add the `.js` extension at the end. With this, we can easily import this function into a file at any depth in our code:

`src/discord/commands/admin/context/test.ts`
```typescript
import { sum } from "#functions"
```
Without this, it would be necessary to use a relative path. See how it would look:

`src/discord/commands/admin/context/test.ts`
```typescript
import { sum } from "../../../../../functions/math.js"
```
In summary, import shortcuts facilitate importing anything and make the code more readable and organized.


