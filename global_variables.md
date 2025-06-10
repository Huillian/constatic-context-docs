## Global Variables

### Useful global variables for Discord bot development

#### Global Variables
There are global constant variables that you can use in method or function option objects, also using the "short syntax."

These are variables with the same name as very common properties when we are creating commands and systems for our Discord bot. When we use these properties, which are usually optional, we define a default value for them.

For example, often when we want an interaction response to be ephemeral, we have to define "Ephemeral" in the `flags` property of the options object:

```typescript
interaction.reply({
  flags: ["Ephemeral"],
  // ...
})
```
The other flags that can be passed in the array of this property are rarely used. With this in mind, we have a global variable ready to be used in this context. Since it is global, it does not need to be imported, thus allowing the "short syntax" to define it:

`command.ts`
```typescript
interaction.deferReply({ flags }); // Contains "Ephemeral" by default;
```
This is the file that contains the global variables:

`src/settings/global.ts`
```typescript
declare global {
  const flags: ["Ephemeral"];
  // ...
}

Object.assign(globalThis, {
  flags: ["Ephemeral"], // Interaction response property
  // ...
});
```
In this way, it is not necessary to import these variables as they have global visibility.

See other examples of using global variables and short syntax:

```typescript
const response = await interaction.reply({ withResponse, /* ... */ });

interaction.reply({ flags, content: "Pong" });

const embed = new EmbedBuilder({
  fields: [
    { name: "Users", value: `${users.size}`, inline },
    { name: "Guilds", value: `${guilds.size}`, inline },
  ]
})
```


