## Autocomplete

### How to create Discord autocomplete commands

#### Creating autocomplete options
Imagine you want to provide choices for a command option dynamically, for example, a command to search for videos on YouTube. You can use the autocomplete option, so based on what the user types in this option, you can search APIs and return a list of choices.

It is necessary to have a slash command to create an autocomplete option.

You can create an autocomplete option in your command and respond to it using the `autocomplete` method on top of `run`.

`command.ts`
```typescript
createCommand({
  	name: "search",
	description: "Search command",
	type: ApplicationCommandType.ChatInput,
	options: [
		{
			name: "term",
			description: "term",
			type: ApplicationCommandOptionType.String,
			autocomplete: true, 
			required: true,
		}
	],
	async autocomplete(interaction) { 
		const focused = interaction.options.getFocused(); 
		const results = await searchData(focused); 
		if (results.length < 1) return; 
		const choices = results.map(data => ({
			name: data.title, value: data.url
		})); 

		return choices; 
	},
	async run(interaction){
		const { options } = interaction;

		const query = options.getString("term", true);
		
		interaction.reply({ flags: ["Ephemeral"], content: query });
	}
});
```
Note that you can only return the options, as the autocomplete command handler will automatically limit the array items to 25.

```typescript
// ...
	return choices; 
// ...
```
See how the internal function of the autocomplete command handler works:

```typescript
// ...
const command = baseStorage.commands.get(interaction.commandName);
if (command && "autocomplete" in command && command.autocomplete){
    const choices = await command.autocomplete(interaction);
    if (choices && Array.isArray(choices)){
        interaction.respond(choices.slice(0, 25));
    }
};
// ...
```
If you have a large number of items, use autocomplete to try to find them:

`command.ts`
```typescript
createCommand({
    // ...
    async autocomplete(interaction) {
		const { options, guild } = interaction;

        const focused = options.getFocused();
        const documents = await db.get(guild.id);

        const filtered = documents.filter(
            data => data.address.toLowerCase().includes(focused.toLowerCase())
        )
        if (filtered.length < 1) return;
        return filtered.map(data => ({
			name: data.title, value: data.url
		}));
	},
    // ...
})
```
If you prefer, you can use the `respond` method of the interaction as you normally would:

```typescript
	return choices; 
	interaction.respond(choices.slice(0, 25))
```


