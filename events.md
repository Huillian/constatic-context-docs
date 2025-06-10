## Events

### How to create Discord event listeners

#### Creating functions for event listeners
To create a function for a Discord event listener, use the `createEvent` function.

First, import from the base:

`src/events/myevent.ts`
```typescript
import { createEvent } from "#base";
```
Use the `name` property to create a custom identifier for your event, then define which Discord event you expect using the `event` property.

```typescript
import { createEvent } from "#base";

createEvent({
    name: "Message edit logs",
    event: "messageUpdate",
    run(oldMessage, newMessage) {
        console.log("Message edited at:", newMessage.editedAt?.toDateString());
        console.log("Author", newMessage.author?.displayName);
        console.log("Old message content: ", oldMessage.content);
        console.log("New message content:", newMessage.content);   
    }
});
```
All Discord events are typed by the `event` property. When you choose an event, the `run` method will be automatically typed with all the arguments that event receives.


