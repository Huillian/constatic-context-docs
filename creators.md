## Creators

### Main base structures for creating commands and systems

In this base, to create commands and systems, you should use the creator functions: `createCommand`, `createEvent`, and `createResponder`.

By executing these functions and passing the respective structure data, they will be created and registered.

You can define some options in the `index` file of the `discord` folder, see:

`src/discord/index.ts`
```typescript
export const { createCommand, createEvent, createResponder } = setupCreators();
```
This function receives options and returns the command, event, and responder creators for you to use. By exporting this return, you can import them at any time using the base shortcut:

```typescript
import { createCommand, createEvent, createResponder } from "#base";
```

### Command Options
You can define some command options in the `setupCreators` function:

*   Default Member Permissions
*   Guilds
*   Middleware
*   onError
*   onNotFound
*   Verbose

Define what will be the default permissions for all commands, if none is defined with the `defaultMemberPermissions` option (it's the same permission array as the command).

```typescript
/* ... */ setupCreators({
    commands: {
        defaultMemberPermissions: ["SendMessages", "Connect"]
    },
});
```
This way, when creating a command and not defining any default member permission, what you defined in the `setupCreators` function will be the default for all commands.

`command.ts`
```typescript
createCommand({
    name: "ping",
    description: "Responds with pong",
    type: ApplicationCommandType.ChatInput,
    // defaultMemberPermissions: [] <-- Not defined!
    // But by default it will be registered in the application with ["SendMessages", "Connect"]
    async run(interaction){
        // ...
    }
})
```

### Responder Options
You can define some responder options in the `setupCreators` function:

*   Middleware
*   onError
*   onNotFound

You can define a middleware function that will be executed before the `run` function of the responders:

```typescript
/* ... */ setupCreators({
    responders: {
        async middleware(interaction, _block, params){
            console.log("Command executed:", interaction.customId);
            console.log("Parameters:", params);
        }
    },
});
```
As with commands, you can do a lot with this, such as displaying standardized logs for all executed responders, injecting additional information into the interaction, or even blocking execution based on checks.

You can execute the `block` function to block the execution of responders.

```typescript
/* ... */ setupCreators({
    responders: {
        async middleware(interaction, block){
            if (interaction.isButton() && blockedUsers.includes(interaction.user.id)){
                interaction.reply({ content: "You cannot click any button!" });
                block();
                return;
            }
        }
    },
});
```

### Event Options
You can define some event options in the `setupCreators` function:

*   Middleware
*   onError

You can define a middleware function that will be executed before the `run` function of the events:

```typescript
/* ... */ setupCreators({
    events: {
        async middleware(event){
            console.log("Event", event.name, "emitted");
        }
    },
});
```
As with commands and responders, you can do a lot with this, such as displaying standardized logs for all executed events, injecting additional information into specific event arguments, or even blocking execution based on checks.

In the first argument, an object with the event data is received, where `name` is the name of the emitted event and `args` are the arguments of that event. Then, by checking the event name, the typing of the arguments is automatically defined:

```typescript
/* ... */ setupCreators({
    events: {
        async middleware(event) {
            if (event.name === "guildMemberUpdate"){
                const [oldMember, newMember] = event.args;
                // ...
                return;
            }
            if (event.name === "guildAuditLogEntryCreate"){
                const [entry, guild] = event.args;
                // ...
                return;
            }
            if (event.name === "messageCreate"){
                const [message] = event.args;
                // ...
                return;
            }
        },
    },
});
```
You can execute the `block` function to block the execution of events.

```typescript
/* ... */ setupCreators({
    events: {
        async middleware(event, block){
            if (event.name === "messageDelete"){
                const [message] = event.args;
                if (message.inGuild() && message.author.id == message.guild.id){
                    block();
                    return;
                }
            }
        }
    },
});
```
In the case of events, it is also possible to pass tags as an argument to the `block` function. This way, you can have 3 events of the same type, for example `messageCreate`, but only block those with previously defined tags:

**Events / Middleware / Utility Functions**

```typescript
createEvent({
    name: "Morning workflow",
    event: "messageCreate",
    tags: ["morning"],
    async run(message) {
        // ...
    },
});

createEvent({
    name: "Night workflow",
    event: "messageCreate",
    tags: ["night"],
    async run(message) {
        // ...
    },
});

createEvent({
    name: "Messages Workflow",
    event: "messageCreate",
    async run(message) {
        // ...
    },
});
```
You can pass as many tags as you want to the `block` function:

```typescript
// ...
block("morning", "night", "foo", "bar", "baz")
// ...
```
All events that contain any of the tags passed as an argument to the `block` function will not be executed.


