# Userscript Browser Ext

Lightweight self-manage plugin for custom userscripts in browser.
This is "paranoid-mode" plugin, so you can easily check everything and know, 
that is sent anywhere.

It uses declarative content scripts feature, read more: https://developer.chrome.com/extensions/content_scripts#declaratively

## Get started

Let's add simple scripts to any google pages.

### Create script
Place new js file to `scripts` directory, for example `scripts/google.js`:

```js
/**
 hello google

 matches: https://google.com/*
 matches: https://*.google.com/*

 run_at: document_start
 */

console.log('Hi, google')
```

This file starts with docstring, which describe where and when we should run this script.

Note: You may also add custom css, by adding css file with the same filename (`scripts/google.css`).

### Build your manifest

Run `./create_manifest` program, it will create your `manifest.json` file.

You can check, that generated manifest contains something like:
```json
{
    ...
    "content_scripts": [
        {
            "matches": [
                "https://google.com/*",
                "https://*.google.com/*"
            ],
            "exclude_matches": [],
            "js": [
                "scripts/google.js"
            ],
            "run_at": "document_start"
        }
    ],
    ...
```

### Install your plugin

First of all enable "Developer Mode" in your browser ([chrome://extensions/](chrome://extensions/) -> toggle on "Developer Mode" at top right corner)

Press "Load unpacked" button and select your plugin root directory (this project root). 

Check that your plugin is installed and enabled.

### Testing

Navigate to [https://google.com](https://google.com), 
open developer console and check that "Hello, google" has written.

## Important information

Google Chrome doesn't automatically reload your scripts, 
so when you change `scripts/google.js` 
you need to click on plugin icon - it will reload the plugin automatically.

When you change docstring, filename or add more scripts 
you need to rebuild `manifest.json` using `./create_manifest`, 
and then reload the plugin.

## Docstring Reference

```js
/**
   <description>

   matches: <match expression>
   matches: <match expression>
   <more match rules>

   exclude_matches: <exclude_matches>
   exclude_matches: <exclude_matches>
   <more exclude_matches rules>

   run_at: <document_idle|document_start|document_end>
   all_frames: <true|false>
 */
```