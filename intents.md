## Intents

### Discord bot intents using Constatic Base

Intents were introduced by Discord so that bot developers can choose which events their bot receives based on what data it needs to function. For a given event to be emitted, a related intent needs to be enabled.

For example, one of the most used events in Discord bots is `guildMemberAdd`. This event is emitted when a member joins the guild, but it is necessary to define the `GuildMembers` intent for the event to be emitted.

When generating this project, by default, all intents are enabled in the Client. See below how a simple code would look without using Constatic Base:

`src/index.ts`
```typescript
import { Client } from "discord.js";

const client = new Client({ 
    intents: [
        "GuildMembers",
        "MessageContent",
        "Guilds",
        "GuildBans",
        "GuildPresences",
        // ...
    ]
});
```
But it is not necessary to define any intent, because by default, all are already enabled:

`src/index.ts`
```typescript
import { bootstrap } from "#base";

await bootstrap({ meta: import.meta });
```
Knowing this, you need to have enabled all intents for your bot application in the Discord developer portal.

However, if you want to define only specific intents, as in the case of a public bot where you don't want to enable the `MessageContent` intent, just use the `intents` property of the `bootstrap` function; it is the same as the Client options:

`src/index.ts`
```typescript
import { bootstrap } from "#base";

await bootstrap({ 
    meta: import.meta,
    intents: [
        "Guilds",
        "GuildWebhooks",
        "GuildBans",
        // ...
    ]    
});
```


