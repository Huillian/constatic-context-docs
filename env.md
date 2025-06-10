## Env

### Loading environment variables natively

#### Env file

**Node / Bun**
With the latest versions of Node.js, we can use the `--env-file` flag to indicate an environment variable file for our project:

```bash
node --env-file .env ./dist/index.js
```

By starting the project with this flag in the script, the `process.env` object will contain all variables defined in the `.env` file at the project root.

You can have two env files in your project and choose which one to use through the predefined scripts:

`package.json`
```json
{
  // ...
  "scripts":{
    "dev": "tsx --env-file .env ./src/index.ts",
    "dev:dev": "tsx --env-file .env.dev ./src/index.ts",
  }
}
```
If you have a `.env.dev` file, you can run the `dev:dev` script:

```bash
npm run dev:dev
```

This is the same for all other scripts:

```bash
npm run start:dev
npm run watch:dev
```
This way, you can have development and production environment variables.

### Validating environment variables
With Zod, we can create a schema for the environment variables object to validate before the project starts, thus ensuring that the information is correct and also providing autocompletion for the variables we have to the intellisense, see:

`src/settings/env.schema.ts`
```typescript
import { z } from "zod";

export const envSchema = z.object({
    BOT_TOKEN: z.string({ description: "Discord Bot Token is required" }).min(1),
    WEBHOOK_LOGS_URL: z.string().url().optional(),
    // Env vars...
});
```
This schema will be used in the file by the function in `src/settings/env.validate.ts` which creates the `env` variable exported in `src/settings/index.ts`:

`src/settings/index.ts`
```typescript
import settings from "../../settings.json" with { type: "json" };
import { envSchema } from "./env.schema.js"; 

import "./global.js";
import { logger } from "./logger.js";
import { validateEnv } from "./env.validate.js"; 
export * from "./error.js";

const env = validateEnv(envSchema); 

export { settings, logger, env };
```
When you need to use the validated and transformed environment variables, just import `env` from `#settings`:

```typescript
import { env } from "#settings";

console.log(env.BOT_TOKEN);
```
You can define the minimum number of characters, the format, whether it should be a URL, email, restrict values, convert to other types. See some examples below:

**URL / Authentication / Connection**
`src/settings/env.schema.ts`
```typescript
import { z } from "zod";

export const envSchema = z.object({
    BOT_TOKEN: z.string({ description: "Discord Bot Token is required" }).min(1),
    DATABASE_URL: z.string({ description: "Database URL is required" }).url(), 
    WEBHOOK_LOGS_URL: z.string().url().optional(),
    // Env vars...
});
```
With this in mind, when working with more tools or libraries that require sensitive information stored in the `.env` file, just add the necessary validations to the schema.


