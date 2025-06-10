## Troubleshooting

### See how to troubleshoot some known errors when trying to use the CLI and Constatic Base

First of all, make sure you have met some requirements to use this project:

*   Node.js 20.11 or higher installed (or Bun)
*   Basic knowledge of programming logic
*   Basic knowledge of JavaScript and TypeScript
*   Basic knowledge of discord.js

If you are facing any of the errors listed below, check out some solutions you can try:

*   `ENOENT: no such file or directory, lstat "C:\Users...\AppData\Roaming\npm"`
*   `Script execution has been disabled on this system`
*   `Slash commands (/) do not appear on the server`
*   `Used disallowed intents`
*   `Cannot find module`

### `ENOENT: no such file or directory, lstat "C:\Users...\AppData\Roaming\npm"`
When trying to run the CLI with the `npx constatic` command, if Node.js does not find the `npm` folder on your computer, you will see this error:

**Solution:** The solution is as simple as possible: just create the folder. The error itself indicates the expected location: `C:\Users\YOURUSER\Roaming\npm`.

Then navigate to your user's `Roaming` folder. The easiest way is to open `Run` (Win + R), type `%appdata%`, and click `OK`.

Once you open this directory, create a folder named `npm`, and it's solved!

### `Script execution has been disabled on this system`
Occurs when you try to execute a PowerShell script, and the script execution policy does not allow it on your system.

Open the start menu and search for `PowerShell` (a program like any other), run it to open the PowerShell terminal, and execute the command below to change the script execution policy:

```bash
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
```
Restart the terminal you used to try to run the CLI with `npx constatic` and run it again.

### `Slash commands (/) do not appear on the server`
You started the project, the terminal showed that the commands were registered, but when trying to use them on the server, they don't appear?

Just restart your client (your Discord application) to refresh the cache. On the computer, just press the shortcut `CTRL + R`, and on the mobile, close the application and open it again.

You can be sure that the commands have been registered in the application by going to `Server Settings` > `Integrations` > `Your Application`, and you will see all the commands there.

This happens when commands are registered in the bot application. If you choose to register commands per guild, they will be updated instantly.

### `Used disallowed intents`
If you just started your application in the terminal and received this error from Discord, it means that you did not enable the intents in the Discord developer portal. Access this mini-guide and complete step 4.

### `Cannot find module`
This error usually occurs when an import statement does not find the corresponding module. Let's talk about two types of modules:

*   **Internal and external modules:** These can be native libraries like `node:fs`, `node:path`, or external ones like `discord.js`, `chalk`.
*   **Local modules:** These are files in your project, like `./index.js`, `../../main.js`, `./functions/math.js`.

If this error occurs displaying the name of a library, you probably misspelled the name or it is not installed as a dependency.

Now, if the error is displaying a path to a file, it is very likely that you specified the path incorrectly or in the wrong way.

The only valid file paths are relative ones (which start with `.`) or import shortcuts (which start with `#`). If your path does not start with either, it is not a valid file path!


