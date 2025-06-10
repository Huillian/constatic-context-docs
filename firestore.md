## Firestore

### See how to use the Firestore database preset

You will need to have a Firestore database to use this preset! If you want to create one for free in the cloud, follow this guide.

### Env
The environment variable validation schema receives a new property; now it is necessary to define the path to your Firebase account file in the `FIREBASE_PATH` variable:

`.env`
```
# ...
FIREBASE_PATH=./firebase.json
# ... 
```

See an example of the `firebase.json` file for the Firestore database:

`firebase.json`
```json
{
    "type": "type",
    "project_id": "project_id",
    "private_key_id": "private_key_id",
    "private_key": "private_key",
    "client_email": "private_key",
    "client_id": "private_key",
    "auth_uri": "private_key",
    "token_uri": "private_key",
    "auth_provider_x509_cert_url": "private_key",
    "client_x509_cert_url": "private_key"
}
```

Below, check the presets according to the library used:

*   Typesaurus
*   Firelord

### Typesaurus

#### Structure
The structure of this preset was made using the Typesaurus library, so to create our data, we use Typesaurus schemas.

The `index.ts` file in the `src/database` folder must make the connection to the database:

`src/database/index.ts`
```typescript
import firebase from "firebase-admin";
import { MemberDocument } from "./documents/MemberDocument.js";
import { GuildDocument } from "./documents/GuildDocument.js";
import { schema, Typesaurus } from "typesaurus";
import { logger, env } from "#settings";
import path from "node:path";
import chalk from "chalk";
import fs from "node:fs";

const firebaseAccountPath = rootTo(env.FIREBASE_PATH);

if (!fs.existsSync(firebaseAccountPath)){
    const filename = chalk.yellow(`"${path.basename(firebaseAccountPath)}"`);
    const text = chalk.red(`The ${filename} file was not found in ${__rootname}`);
    logger.error(text);
    process.exit(0);
}

const firebaseAccount: firebase.ServiceAccount = JSON.parse(
    fs.readFileSync(firebaseAccountPath, { encoding: "utf-8" })
);


firebase.initializeApp({ credential: firebase.credential.cert(firebaseAccount) });

export const db = schema(({ collection }) => ({
    guilds: collection<GuildDocument>().sub({
        members: collection<MemberDocument>()
    })
}));

export type DatabaseSchema = Typesaurus.Schema<typeof db>;
export type MemberSchema = DatabaseSchema["guilds"]["sub"]["members"]["Data"];
export type GuildSchema = DatabaseSchema["guilds"]["Data"];

export * from "./functions/guilds.js";
export * from "./functions/members.js";
```
The document collections are in the `db` variable object, meaning that for each new document schema you create, place the collection as a property of this variable or as a sub-property of an existing one.

So, when you need to create, read, update, or delete data, just import the `db` variable from `#database` and use the collection methods:

Check the library documentation for best usage: https://typesaurus.com/get-started/

Always use the `db` variable from the import shortcut `"#database"`; this way, the file containing the code that connects to the database is called.

#### Interfaces
To have autocomplete for properties in the `db` variable when creating collections, it is necessary to pass data interfaces in the generic of the collections. You can create the interfaces for your collections in the `src/databases/documents` folder.

*   Member collection interface
*   Guild collection interface

`src/database/documents/member.ts`
```typescript
interface MemberDocument {
    wallet?: {
        coins?: number;
    }
}

export { MemberDocument };
```

After creating your interface, import it into the `index.ts` file in the `src/database` folder and create a collection passing it as a generic:

`src/database/index.ts`
```typescript
import { MemberDocument } from "./documents/MemberDocument.js";
import { GuildDocument } from "./documents/GuildDocument.js";
import { schema } from "typesaurus";

export const db = schema(({ collection }) => ({
    guilds: collection<GuildDocument>().sub({ 
        members: collection<MemberDocument>() 
    })
}));
```


