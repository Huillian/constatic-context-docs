## Slash Commands

### How to create Discord slash commands

#### Creating Slash Commands
First of all, import the `createCommand` function from the base and `ApplicationCommandType` from `discord.js`.

`ping.ts`
```typescript
import { createCommand } from "#base"
import { ApplicationCommandType } from "discord.js";
```
See conventions for creating your commands.

To create a slash command, you must define the name, description, and type:

`ping.ts`
```typescript
createCommand({
    name: "ping",
    description: "Ping command",
    type: ApplicationCommandType.ChatInput,
    async run(interaction) {
        interaction.reply({ flags: ["Ephemeral"], content: "Pong" });
    },
});
```
Command names (slash) and command option names cannot be empty strings, cannot contain special characters, uppercase letters, or spaces.

### Defining command options
You can also define options, subcommands, and groups.

`manage.ts`
```typescript
import { createCommand } from "#base";
import { ApplicationCommandOptionType, ApplicationCommandType } from "discord.js";

createCommand({
    name: "manage",
    description: "Manage command",
    type: ApplicationCommandType.ChatInput,
    options: [ 
        { 
            name: "users", 
            description: "Manage users", 
            type: ApplicationCommandOptionType.Subcommand,
            options: [ 
                { 
                    name: "user", 
                    description: "Select the user", 
                    type: ApplicationCommandOptionType.User,
                    required: true
                } 
            ], 
        } 
    ], 
    async run(interaction) {
        const { options } = interaction;

        switch(options.getSubcommand(true)){
            case "users":{
                const user = options.getUser("user", true);
                interaction.reply({ flags: ["Ephemeral"], content: `${user} managed` })
                return;
            }
        }
    },
});
```

### Global
The `global` option in commands is useful when you want the command to be registered in the application instead of the guild. It is only necessary when guild IDs are defined in the `setupCreators` function.

The code below makes commands registered only in the valid guilds listed in the array:

`src/discord/index.ts`
```typescript
import { setupCreators } from "#base";

export const { createCommand, createEvent, createResponder } = setupCreators({
    commands: {
        guilds: [
            process.env.MAIN_GUILD_ID,
            process.env.DEV_GUILD_ID,
            process.env.TEST_GUILD_ID,
        ]
    }
});
```
Now see this code where some commands are created:

```typescript
createCommand({
	name: "settings",
	description: "Server settings command",
	async run(interaction){
		// ...
	}
});

createCommand({
	name: "manage",
	description: "Command to manage members, channels and roles",
	async run(interaction){
		// ...
	}
});

createCommand({
	name: "info",
	description: "Displays bot information",
	global: true, 
	async run(interaction){
		// ...
	}
});
```
The `settings` and `manage` commands will be registered in the guilds defined in the `setupCreators` function, but the `info` command will be registered globally in the application, thus being available in any guild.


