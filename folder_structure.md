## Structure

### How the project folders are structured

#### Folder Structure
If you follow the structure established by this project, it will be very easy to get support anywhere, not to mention that your code will be organized and also very semantic.

See below an overview of how the project is structured:

```
.vscode
src
.env
.gitignore
README.md
package.json
tsconfig.json
settings.json
```

All the following folders are inside `src`:

#### `discord` folder
The structures of this base, such as the `createCommand`, `createEvent`, and `createResponder` functions, need to be in a file that is imported before the bot starts. Everything in the `src/discord` folder and its subfolders will be imported before the bot starts, thus loading all structures.

*   **Commands:** Create all your commands in the `src/discord/commands` directory.
*   **Events:** Create all your events in the `src/discord/events` directory.
*   **Responders:** Create all your responders in the `src/discord/responders` directory.

*   **Functions:** Create all your functions in the `src/functions` directory.

#### `settings` folder
This contains essential files for the project's operation, such as environment variable schema definitions, global variables, error handlers, typing files, etc.

#### `database` folder
This folder contains all the configuration and models for the database chosen during project generation. See more [here](#).

#### `server` folder
This is where the HTTP request server chosen during project generation is started. See more [here](#).


