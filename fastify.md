## Fastify

The API server preset using Fastify comes with some configurations to facilitate development. See below what you need to know to use it:

It is necessary to know the Fastify framework to use it in the best way; read the documentation: https://fastify.dev/

### Entry Function
Just as in the project base we have the `bootstrap` function to load modules and start the bot, we also have the `startServer` function located in the `src/server/index.ts` file. This function is exported to be called as soon as the bot is ready:

`src/index.ts`
```typescript
import { bootstrap } from "#base";
import { startServer } from "#server";

await bootstrapApp({ 
    meta: import.meta,
    whenReady: startServer
});
```
If you want to execute other things in the `whenReady` method.
With this, the Fastify server will start right after the bot comes online, receiving the `Client` as an argument.

### Environment Variables
The Zod schema for validating the environment is changed to allow new environment variables:

`src/settings/env.ts`
```typescript
import { z } from "zod";

const envSchema = z.object({
    BOT_TOKEN: z.string({ description: "Discord Bot Token is required" }).min(1),
    WEBHOOK_LOGS_URL: z.string().url().optional(),
    SERVER_PORT: z.number({ coerce: true }).optional(), 
    // Env vars...
});

type EnvSchema = z.infer<typeof envSchema>;

export { envSchema, type EnvSchema };
```
This means you can change the Fastify server PORT using this variable in the `.env` file:

`.env`
```
BOT_TOKEN=yourtoken
SERVER_PORT=8080
```

### Automatic Loading and Registration
By default, this preset comes with the `@fastify/autoload` plugin installed, which allows us to configure a folder with route handlers that will be automatically defined:

`src/server/index.ts`
```typescript
import fastify, { type FastifyInstance } from "fastify";
import autoload from "@fastify/autoload";
import type { Client } from "discord.js";
import path from "node:path";
// ...

export async function startServer(client: Client<true>){
    // ...
    app.register(autoload, { 
        dir: path.join(import.meta.dirname, "routes"), 
        routeParams: true, 
        options: client, 
    }); 
    // ...
}
```
This configuration will import and register the route handlers from all files in the `src/server/routes` folder.

Therefore, it is very important that all files in the `routes` folder have a default export of a function that receives the Fastify instance, the Discord client, and the Fastify `done` function.

See an example of the root route returning some information:

`src/server/routes/home.ts`
```typescript
import { defineRoutes } from "#server";
import { StatusCodes } from "http-status-codes";

export default defineRoutes((app, client) => {
    app.get("/", (_, res) => {
        return res.status(StatusCodes.OK).send({
            message: `🍃 Online on discord as ${client.user.username}`,
            guilds: client.guilds.cache.size,
            users: client.users.cache.size
        });
    });
});
```

### Other ways to define routes
The `@fastify/autoload` plugin imports modules from subfolders and transforms the path into a route:

Create a file in `src/server/routes/users`:

`src/server/routes/users/route.ts`
```typescript
import { defineRoutes } from "#server";
import { StatusCodes } from "http-status-codes";

export default defineRoutes((app, client) => {
    app.get("/", (_, res) => {
        return res.status(StatusCodes.OK).send({
            message: "Returns all users",
        });
    });

    app.get("/:id", (req, res) => {
        const { id } = req.params as { id: string };

        return res.status(StatusCodes.OK).send({
            message: "Returns a user by id",
        });
    });
});
```
In the example above, two routes will be registered: the first being `/users` and the second being `/users/:id`.

Read the full plugin documentation to understand the best use cases: https://github.com/fastify/fastify-autoload

### Cors
In this preset, the CORS plugin is already installed and has a basic configuration:

`src/server/index.ts`
```typescript
import fastify, { type FastifyInstance } from "fastify";
import cors from "@fastify/cors";
import type { Client } from "discord.js";
import path from "node:path";
// ...

export async function startServer(client: Client<true>){
    // ...
    app.register(cors, { origin: "*" }); 
    // ...
}
```


