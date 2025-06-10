## PM2

### How to host the Discord bot project using PM2

PM2 is a daemon process manager that will help you manage and keep your application online.

To get started, install PM2 globally:

```bash
npm install -g pm2
```

Create a file in the project root to start the `start` script. Name it `pm2.start.mjs`:

`pm2.start.mjs`
```javascript
import { exec } from "node:child_process";
exec("npm run start", { windowsHide: true });
```

Make sure the project is built.

Then you can start your application using this command:

```bash
pm2 start --name my-bot ./pm2.start.mjs
```
You can set a custom name for your process using the `--name` flag.

Done, your application will run in the background on the machine.

Below are some useful commands you can use to manage your process:

| Command | Usage | Description |
|---|---|---|
| `list` | `pm2 list` | Displays all registered processes |
| `stop` | `pm2 stop my-bot` | Stops your application's process |
| `restart` | `pm2 restart my-bot` | Restarts your application's process |
| `logs` | `pm2 logs my-bot` | Displays your application's process logs |
| `delete` | `pm2 delete my-bot` | Deletes your application's process |


