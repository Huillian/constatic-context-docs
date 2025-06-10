## Context Menus

### How to create Discord context menu commands

#### Creating Context Menus
Context menus do not have a description, and their names can contain uppercase letters and spaces.

*   User context menu
*   Message context menu

To create a user context menu command, you need to define `name` and `type`.

```typescript
createCommand({
    name: "Profile",
    type: ApplicationCommandType.User,
    async run(interaction) {
        const { targetUser } = interaction;
        interaction.reply({ flags: ["Ephemeral"], content: `${targetUser}"s profile` });
    },
});
```


