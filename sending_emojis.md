## Sending Emojis

### Send images to create application emojis practically

To access this option, you must have selected an existing application or entered an application token previously. Find more details [here](#).

### Sending Emojis
By selecting the `Send` option in the emoji tool menu, you can provide a path, relative to where the CLI was executed, to a folder containing image files. The CLI will then filter all image files that meet the requirements to become emojis:

*   **File type:** JPEG, PNG, GIF, WEBP, AVIF
*   **Maximum file size:** 256 KB
*   **Recommended dimensions:** 128 x 128
*   **Naming:** Emoji names must be at least 2 characters long and can only contain alphanumeric characters and underscores.

#### Path to images
The CLI will ask for the path, so it is recommended that you run it with the terminal open in the image folder:

Consider this folder example:

```
icons/
├── dashboard/
│   ├── home.png
│   ├── settings.jpg
│   ├── stats.gif
│   └── users.png
├── economy/
│   ├── coin.png
│   ├── wallet.jpg
│   ├── shop.gif
│   └── bank.png
└── navigation/
    ├── arrow_left.png
    ├── arrow_right.jpg
    ├── menu.gif
    └── close.png
```

By running the CLI with the terminal open in the `icons` folder, you can pass the relative path to the folders containing images that will become emojis:

```
Enter the image directory path
> ./economy
```

This way, the CLI will pick up the `coin.png`, `wallet.jpg`, `shop.gif`, and `bank.png` files. They will be sent to the Discord API to create emojis for the selected application.

Or, if you prefer to send all images from all subfolders, you can provide the current directory:

```
Enter the image directory path
> ./
```

This way, the CLI will search for images in all folders and subfolders of this directory.

The file name will become the emoji name, so consider naming the files according to the Discord requirements mentioned at the beginning of this page and also giving unique names to each image file in different folders.

### Overwrite methods
If your application already has emojis, some of them might have the same name as the new ones you are sending. With this in mind, you can choose from 3 overwrite methods:

*   ✍️ Overwrite if exists
*   ❔ Ask before overwriting
*   🔏 Skip and do not overwrite

After selecting one of the methods, the emoji sending process will begin. They will be sent one by one to the Discord API to avoid Rate Limit Abuse, so you will have to wait for the process to complete.

When the process is complete, all emojis will be available in your application and ready for use.


