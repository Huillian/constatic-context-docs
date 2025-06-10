## Discloud

### How to host the Discord bot project on Discloud hosting

Discloud is a cloud application hosting service.

Make sure the project is built.

When generating the project using the CLI, you can select the `discloud project` feature, which will include the two necessary files to send your application to the hosting.

But... If you didn't check that option, here's how the files should look:
In the `discloud.config` file, you can define a name and an avatar for your application on the hosting if you prefer.

`discloud.config`
```
NAME=My Awesome Bot
AVATAR=https://i.imgur.com/L7Mxhoj.gif
# ...
```

Install the Discloud extension in Visual Studio Code.

**Step 1:** Click on the Discloud tab on the side of Visual Studio Code and enter your Discloud API token. Then just click on `Upload Discloud` in the bottom bar of Visual Studio Code.

**Step 2:** If your project has no errors, it will start in a few moments.

If you don't want to use the Visual Studio Code extension, there are other ways to host, and you can check them here. Just remember to follow the requirements below:

*   The folder that should be sent is `build`, not `src` (that's why it needs to be built first).
*   The files and folders that should be sent are `build`, `.env`, `package.json`, and `discloud.config`.
*   Do not send the `node_modules` folder.
*   It is necessary to define the start script as `npm run start`.


