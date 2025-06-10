## Modals

### How to respond to Discord modals with the Responder structure

#### Creating Responder for modals
Below you can see a code snippet responding to a command interaction by displaying a modal:

`command.ts`
```typescript
import { createModalFields } from "@magicyan/discord";

// ...
interaction.showModal({
    customId: "form/modal",
    title: "Form",
    components: createModalFields({
        name:{
            label: "What is your name?",
            style: TextInputStyle.Short
        },
        age:{
            label: "What is your age?",
            style: TextInputStyle.Short
        },
    })
});
// ...
```
To respond to a modal with the Responder, add `ResponderType.Modal` to the types:

`responder.ts`
```typescript
createResponder({
    customId: "form/modal",
    types: [ResponderType.Modal], cache: "cached",
    async run(interaction) {
        const { fields, member } = interaction;
        const name = fields.getTextInputValue("name");
        const age = fields.getTextInputValue("age");

        await registerMember(member, { name, age }); // Example function

        interaction.reply({ flags: ["Ephemeral"], content: `Registered as ${name}` });
    },
});
```
If the modal is displayed from a component:

`responder.ts`
```typescript
createResponder({
    customId: "open/form",
    types: [ResponderType.Button], cache: "cached",
    async run(interaction) {
        await interaction.showModal({
            customId: "form/modal",
            title: "Form",
            components: createModalFields({
                name:{
                    label: "What is your name?",
                    style: TextInputStyle.Short
                },
                age:{
                    label: "What is your age?",
                    style: TextInputStyle.Short
                },
            })
        });
    },
});
```
You can use the `ResponderType.ModalComponent` type:

`responder.ts`
```typescript
createResponder({
    customId: "form/modal",
    types: [ResponderType.ModalComponent], cache: "cached",
    async run(interaction) {
        // ...
    },
});
```
If you create code that can display the same modal from both a command and a component, simply include both types in the Responder's types array:

**Command / Component**

```typescript
import { myCustomModals } from "#modals" // Example

createCommand({
    name: "form",
    async run(interaction) {
        await interaction.showModal(
            myCustomModals.formModal()
        );
    },
});
```

`responder.ts`
```typescript
createResponder({
    customId: "form/modal",
    types: [
        ResponderType.Modal,
        ResponderType.ModalComponent
    ], cache: "cached",
    async run(interaction) {
        // ...
    },
});
```


