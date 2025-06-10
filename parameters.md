## Parameters

### Transform component and modal customIds into HTTP routes with parameters

#### What are CustomID params?
Let's go a little beyond the conventional. In this base, a feature present in several API server tools has been implemented to allow more advanced systems to be developed with more practicality.

Similar to HTTP routes, we can pass parameters to the customIds of buttons, select menus, and modals. See how simple it is:

First, let's create a user context command:

`src/discord/commands/context/manage.ts`
```typescript
createCommand({
    name: "Manage",
    type: ApplicationCommandType.User,
    async run(interaction){
        const { targetUser } = interaction;

        const embed = new EmbedBuilder({ description: `Manage ${targetUser}` });
        const row = createRow(
            new ButtonBuilder({ 
                customId: `manage/user/${targetUser.id}/kick`, 
                label: "Kick", style: ButtonStyle.Secondary 
            }),
            new ButtonBuilder({ 
                customId: `manage/user/${targetUser.id}/ban`, 
                label: "Ban", style: ButtonStyle.Danger 
            }),
            new ButtonBuilder({ 
                customId: `manage/user/${targetUser.id}/timeout`, 
                label: "Timeout", style: ButtonStyle.Danger 
            }),
            new ButtonBuilder({ 
                customId: `manage/user/${targetUser.id}/alert`, 
                label: "Alert", style: ButtonStyle.Primary 
            })
        );

        interaction.reply({ flags: ["Ephemeral"], embeds: [embed], components: [row] });
    }
});
```
Notice that the `customId` of all buttons has a similarity; both follow a pattern where each segment is separated by `/` (slash).
With this, we can create a Responder that expects any button component that follows this pattern in the `customId`.

It should start with `manage/user`, then it should have two more parameter segments separated by `/` and starting with `:`, where the first will be the user ID and the second will be an action.

It would look like this: `manage/user/:userId/:action`

Then we can define this pattern, and any button whose `customId` follows it will be responded to by the defined function. You can get the parameters in the second argument of the function.

`src/discord/responders/manage.ts`
```typescript
// Dynamic button component function
createResponder({
    customId: "manage/user/:userId/:action",
    types: [ComponentType.Button], cache: "cached",
    async run(interaction, params) {
        const { action, userId } = params;
        const targetMember = await interaction.guild.members.fetch(userId);

        switch(action){
            case "kick": {
                targetMember.kick();
                // do things ...
                break;
            }
            case "ban": {
                targetMember.ban();
                // do things ...
                break;
            }
            case "timeout": {
                targetMember.timeout(60000);
                // do things ...
                break;
            }
            case "alert": {
                targetMember.send({ /* ... */ });
                // do things ...
                break;
            }
        }
    },
});
```
You can use this feature with any type of Responder, but don't forget that Discord has a 100-character limit on `customIds`.

### Transforming CustomID parameters
The `createResponder` function has an option where you can specify a way to transform the parameters object, where initially both the keys and values are only strings. Check it out:

*   Simple transformation
*   Transforming dates
*   Optional transformation
*   Zod schema

Transform the `value` parameter into a number, allowing you to use numeric functions.

**Button**
```typescript
new Button({
    customId: `count/1`,
    label: "Count", 
    style: ButtonStyle.Primary 
})
```

**Responder**
```typescript
createResponder({
    customId: "count/:value", cache: "cached",
    types: [ComponentType.Button],
    parse: params => ({ 
        value: Number(params.value) 
    }), 
    async run(interaction, { value }) {
        console.log(value + 1); // 2
        console.log(value.toFixed(2)); // "1.00"
    }
});
```

### Wildcards
You can also use wildcards if you want to respond to routes with more or fewer segments.

**Using wildcard `*`:**

Use `**` to represent any segment pattern after the `/` (slash).

**CustomIds that will be responded to:**

*   `"giveway"`
*   `"giveway/users"`
*   `"giveway/gifts/nitro"`
*   `"giveway/gifts/account/creator/expiresAt"`

**Responder**
```typescript
createResponder({
    customId: "giveway/**", cache: "cached",
    types: [ComponentType.Button],
    async run(interaction, { _ }) {
        // giveway _: ""
        // giveway/users _: "users"
        // giveway/gifts/nitro _:"gifts/nitro"
        // giveway/gifts/account/creator/expiresAt _:"gifts/account/creator/expiresAt"
    }
});
```

**Named wildcards:**

Give a name to the wildcard instead of `_`. Use `:` to define a name:

**CustomIds that will be responded to:**

*   `"giveway/users"`
*   `"giveway/gifts/nitro"`
*   `"giveway/gifts/account/creator/expiresAt"`

**Responder**
```typescript
createResponder({
    customId: "giveway/**:args", cache: "cached",
    types: [ComponentType.Button],
    async run(interaction, { args }) {
        // giveway/users args: "users"
        // giveway/gifts/nitro args:"gifts/nitro"
        // giveway/gifts/account/creator/expiresAt args:"gifts/account/creator/expiresAt"
    }
});
```
By defining a name for the wildcard, the `customId` now becomes mandatory to have one more segment, meaning it will not respond to just `giveway`, only `giveway/args`...

**Transforming wildcards:**

You can use the `parse` method to transform the wildcard into whatever you want, for example, an array.

**CustomIds that will be responded to:**

*   `"giveway"`
*   `"giveway/users"`
*   `"giveway/gifts/nitro"`
*   `"giveway/gifts/account/creator/expiresAt"`

**Responder**
```typescript
createResponder({
    customId: "giveway/**", cache: "cached",
    types: [ComponentType.Button],
    parse: params => ({
        args: params._.split("/").filter(Boolean)
    }),
    async run(interaction, { args }) {
        // giveway args: []
        // giveway/users args: ["users"]
        // giveway/gifts/nitro args: ["gifts", "nitro"]
        // giveway/gifts/account/creator/expiresAt args: ["gifts", "account", "creator", "expiresAt"]
    }
});
```

The examples on this page were all with buttons, but this feature can be used with any type of responder!


