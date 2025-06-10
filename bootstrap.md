## Bootstrap

### Initial function of the Discord bot project

#### Entry Point
The `index.ts` file in the `src` folder is the entry point of this project. It is where the `bootstrap` function is executed to start the Discord bot application.

`src/index.ts`
```typescript
import { bootstrap } from "#base";

await bootstrap({ meta: import.meta });
```
In this topic, you will check out some interesting options for this function.

#### Meta
This option should receive the `import.meta` of the current file. When running the project in development, the `import.meta` directory will be `src`, but when the project is built, it will be `build`. This option is important so that the files in the `discord` folder can be imported before the bot starts.

#### Directories
You can pass an array of paths relative to the current folder that you want to be imported before the bot starts.

All nested subfolders of the directories you pass in this property will also be loaded, no matter the depth.

`src/index.ts`
```typescript
await bootstrap({ 
    meta: import.meta,
    directories: ["./MyCommands", "./alt/events"]
});
```
With this, you can create commands, events, and responders within these directories, and they will be loaded before the bot starts.

#### Load Logs
It is possible to disable the loading logs of this base's structures by setting `loadLogs` to `false` in the `bootstrap` function.

`src/index.ts`
```typescript
await bootstrap({ 
    meta: import.meta,
    loadLogs: false
});
```

#### When Ready
The base already has a listener for the bot's `ready` event. If you don't want to create a new listener for this, you can use it by simply passing a function to this option.

`src/index.ts`
```typescript
await bootstrap({ 
    meta: import.meta,
    whenReady(client) {
        console.log(`Online as ${client.user.displayName}`)
    },
});
```

#### Before Load
You can execute code before the directories are loaded and receive the client before it goes online (ready).

`src/index.ts`
```typescript
await bootstrap({ 
    meta: import.meta,
    beforeLoad(client) {
        console.log(client.application) // null;
        console.log("This occurs before loading the directories")
    },
});
```


