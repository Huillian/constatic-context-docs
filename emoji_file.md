## Emoji File

### Generate a JSON file containing information about all emojis in your application

To access this option, you need to have selected an existing application or entered an application token previously. Find more details [here](#).

### Emoji File
With the `Generate emoji file` option, you can obtain a JSON file with all the names and IDs of your application's emojis so that you can use them in the texts and components of your bot project through utility functions.

You can specify the file name and path (by default, it will be `emojis.json`). But you can choose another name for the file, like `emojis.dev.json`, or even pass the path to a folder like `./assets/emojis.json`.

Immediately after, you can choose the mode in which the emojis will be listed in the file:

*   **IDs:** The values of all properties are just the emoji IDs.
*   **URLs:** The values of all properties are the emoji CDN URLs.

The example below illustrates what the generated emoji file looks like (fictitious names and IDs).

**IDs / URLs**
`emojis.json`
```json
{
    "animated": {
        "bell": "1069586362549927988",
        "celebration": "1069586365649391617",
        "dots": "1069586367612321792",
        "fatal": "1069586369831243858",
        "refresh": "1069586371079069010"
    },
    "static":{
        "users": "1069586386985812032",
        "danger": "1069586628910270884",
        "info": "1069586629894869037",
        "success": "1069586631614529566",
        "warning": "1069586632520503411",
        
    }
}
```


