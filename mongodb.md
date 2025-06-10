## MongoDB

### See how to use the MongoDB database preset

You will need to have a MongoDB database to use this preset! If you want to create one for free in the cloud, follow this guide.

### Env
The environment variable validation schema receives a new property; now it is necessary to define the URI of your database in the `MONGO_URI` variable:

`.env`
```
# ...
MONGO_URI=uri
# ... 
```

See an example of the format of a MongoDB database URI:

`mongodb+srv://<user>:<password>@<database>.subdomain.mongodb.net`

### Structure
The structure of this preset was made using the Mongoose library, so to create our data, we use Mongoose schemas.

The `index.ts` file in the `src/database` folder must make the connection to the database:

`src/database/index.ts`
```typescript
import mongoose, { InferSchemaType, model } from "mongoose";
import { guildSchema } from "./schemas/guild.js";
import { memberSchema } from "./schemas/member.js";
import { logger, env } from "#settings";
import chalk from "chalk";

try {
   await mongoose.connect(env.MONGO_URI, { dbName: "database" });
   logger.success(chalk.green("MongoDB connected"));
} catch(err){
   logger.error(err);
   process.exit(1);
}

export const db = {
   guilds: model("guild", guildSchema, "guilds"),
   members: model("member", memberSchema, "members")
};

export type GuildSchema = InferSchemaType<typeof guildSchema>;
export type MemberSchema = InferSchemaType<typeof memberSchema>;
```
The document models are in the `db` variable object, meaning that for each new document schema you create, place the model as a property of this variable.

So, when you need to create, read, update, or delete data, just import the `db` variable from `#database` and use the model methods:

`src/discord/commands/members.ts`
```typescript
import { createCommand } from "#base";
import { db } from "#database"; 
import { ApplicationCommandType } from "discord.js";

createCommand({
    name: "members",
    description: "Query all members' documents",
    type: ApplicationCommandType.ChatInput,
    async run(interaction){
        const documents = await db.members.find(); 

        for(const doc of documents){
            console.log(doc.id);
            console.log(doc.guildId);
        }

        // ...
    }
});
```
Always use the `db` variable from the import shortcut `"#database"`; this way, the file containing the code that connects to the database is called.

### Schemas
Create your Mongoose schemas in the `src/database/schemas/` folder for better organization. See the example schemas that come by default:

*   Member model schema
*   Guild model schema

`src/database/schemas/member.ts`
```typescript
import { Schema } from "mongoose";
import { t } from "../utils.js";

export const memberSchema = new Schema(
    {
        id: t.string,
        guildId: t.string,
        wallet: {
            coins: { type: Number, default: 0 },
        }
    },
    {
        statics: {
            async get(member: { id: string, guild: { id: string } }) {
                const query = { id: member.id, guildId: member.guild.id };
                return await this.findOne(query) ?? this.create(query);
            }
        }
    },
);
```
After creating your schema, import it into the `index.ts` file in the `src/database` folder and create a model for it in the `db` variable object:

`src/database/index.ts`
```typescript
import /* ... */ { /* ... */ model } from "mongoose";
import { guildSchema } from "./schemas/guild.js";
import { memberSchema } from "./schemas/member.js";

// ...

export const db = {
    guilds: model("guild", guildSchema, "guilds"), 
    members: model("member", memberSchema, "members") 
};

// ...
```
If you need the types of the document properties, extract the type from the schemas:

`src/database/index.ts`
```typescript
import /* ... */ { /* ... */ InferSchemaType } from "mongoose";
import { guildSchema } from "./schemas/guild.js";
import { memberSchema } from "./schemas/member.js";

// ...

export type GuildSchema = InferSchemaType<typeof guildSchema>;
export type MemberSchema = InferSchemaType<typeof memberSchema>;
```


