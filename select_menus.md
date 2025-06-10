## Select Menus

### How to respond to Discord select menus with the Responder structure

#### Creating Responder for select menus
It is possible to create a Responder that can accept various types of select menus. See an example:

`command.ts`
```typescript
// ...
const row = createRow(
    new StringSelectMenuBuilder({
        customId: "select/fruits",
        placeholder: "Select fruits",
        options: [
            { emoji: "🍎", label: "Apple", value: "apple" },
            { emoji: "🍉", label: "Watermelon", value: "melon" },
            { emoji: "🍊", label: "Orange", value: "orange" }
        ]
    })
);
interaction.reply({ flags: ["Ephemeral"], components: [row] });
// ...
```
Above is the code to send a common select menu as a command response. Below, you can see a Responder with `ResponderType.StringSelect` in the types, so the `run` function of this Responder will be executed when someone selects an option in this select menu:

`responder.ts`
```typescript
createResponder({
    customId: "select/fruits",
    types: [ResponderType.StringSelect], cache: "cached",
    async run(interaction) {
        const selected = interaction.values[0];
        interaction.update({ flags: ["Ephemeral"], content: `${selected} Selected`, components: [] });
    },
});
```
You can combine different types of select menus:

`responder.ts`
```typescript
createResponder({
    customId: "ban/user", cache: "cached",
    types: [
        ResponderType.StringSelect,
        ResponderType.UserSelect,
    ],
    async run(interaction) {
        const { guild, values: [selected] } = interaction;

        await interaction.update({});

        if (interaction.isUserSelectMenu()){
            const member = guild.members.cache.get(selected);
            await member.ban(); 
        } else {
            const member = guild.members.cache.find(m => m.user.username === selected);
            await member.ban(); 
        }

        await interaction.editReply({ 
            flags: ["Ephemeral"], components: [] ,
            content: `${userMention(selected)} was banned!`, 
        });
    },
});
```
Below are all types of select menus:

| Select Menu | ResponderType |
|---|---|
| Strings | `ResponderType.StringSelect` |
| Users | `ResponderType.UserSelect` |
| Channels | `ResponderType.ChannelSelect` |
| Roles | `ResponderType.RoleSelect` |
| Mentionables | `ResponderType.MentionableSelect` |

It is possible to combine with any type. This way, you can have a button, a select menu, and a modal with the same `customId`, and all will be responded to in this function; just perform checks:

`responder.ts`
```typescript
createResponder({
    customId: "menus/main", cache: "cached",
    types: [
        ResponderType.Button,
        ResponderType.StringSelect,
        ResponderType.ModalComponent,
    ],
    async run(interaction) {
        switch (true) {
            case interaction.isButton():
                console.log("Button")
                return;
            case interaction.isStringSelect():
                console.log("Select menu", interaction.values);
                return;
            default:
                console.log("Modal", interaction.fields);
                return;
        }
    },
});
```


