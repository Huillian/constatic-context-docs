## Responders

### Understand how the Responder class works

#### What are Responders?
The `createResponder` function is powerful for handling different types of Discord interactions. With it, we can respond to buttons, select menus, and modals, or all at once.

To create a Responder, you need to import the function and the enum from the base:

`responder.ts`
```typescript
import { createResponder, ResponderType } from "#base";
```
See a simple example below. We will send a button through a command and respond to it using the `createResponder` function.

`command.ts`
```typescript
import { createCommand } from "#base";
import { createRow } from "@magicyan/discord";
import { ApplicationCommandType, ButtonBuilder, ButtonStyle } from "discord.js";

createCommand({
    name: "ping",
    description: "Ping command",
    dmPermission: false,
    type: ApplicationCommandType.ChatInput,
    async run(interaction){
        const row = createRow(
            new ButtonBuilder({
                customId: "ping/button",
                label: "Ping", 
                style: ButtonStyle.Success
            })
        );
        interaction.reply({ flags: ["Ephemeral"], components: [row] });
    }
});
```

`responder.ts`
```typescript
import { createResponder, ResponderType } from "#base";

createResponder({
    customId: "ping/button",
    types: [ResponderType.Button], cache: "cached",
    async run(interaction) {
        interaction.reply({ flags: ["Ephemeral"], content: "pong" });
    },
});
```
This way, we are responding to a fixed button component in a message where the `customId` is `ping/button`. So, any button our bot sends that has this `customId` will be responded to by this function defined in the `run` of our Responder.

Responders use the `interactionCreate` event, which means that even if the bot restarts, if someone uses that button again, it will still be responded to with the defined function. The same applies to select menus and modals.


