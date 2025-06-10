## Bot Application

### How to create a Discord bot application, enable intents, and get the token

This is a simple guide on how to create an application to develop Discord bots.

You need to set up a Discord bot application through the Discord website.

First, go to https://discord.com/developers/applications

#### Create a new application

**Step 1:** Define the application name and click `Create`.

**Step 2:** Enable the intents.

**Step 3:** Your bot's token will be revealed when you press the "Reset Token" button and confirm.

**Step 4:** Copy the token and paste it into the `.env` file.

`.env`
```
BOT_TOKEN=yourtoken
```

To invite the bot to the server, click on the `OAuth2` tab and then on `URL Generator`. Check the "bot" and "application.commands" scopes. Check the administrator permission. A URL will be generated below; just access it to choose the server you want the bot to join.

**Step 5:** (Image of selecting scopes)

**Step 6:** (Image of generated URL and inviting bot)


