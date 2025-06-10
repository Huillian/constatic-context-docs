## Buttons

### How to respond to Discord buttons with the Responder structure

Respond to a button by setting the Responder type to button.

`command.ts`
```typescript
const row = createRow(
    new ButtonBuilder({
        customId: "confirm/button", 
        label: "Confirm", 
        style: ButtonStyle.Success
    })
);
interaction.reply({ flags: ["Ephemeral"], components: [row] });
```

`responder.ts`
```typescript
createResponder({
    customId: "confirm/button",
    types: [ResponderType.Button], cache: "cached",
    async run(interaction) {
        interaction.update({ 
            flags: ["Ephemeral"], content: "Confirmed", 
            components: [] 
        });
    },
});
```


