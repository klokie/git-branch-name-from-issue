# attempt to parse Jira or Github issue from tab title
## for Chrome & Firefox

Do you often have to manually create a git branch name from an open issue in Jira or Github?

based on the wonderful [Copy as Markdown](https://github.com/yorkxin/copy-as-markdown) browser extension.
### Firefox

Please refer to this Firefox Help: https://support.mozilla.org/en-US/kb/manage-extension-shortcuts-firefox

### Chrome

The Keyboard Shortcuts of extensions can be found at `chrome://extensions/shortcuts` URL. (Paste and open the link in the Location Bar).

## Known Issues


## Development

Here is the folder structure. Platform-specific folder is used to resolve browser inconsistencies.

```
src/               # Shared Source Code
  background.js
  ...
chrome/            # Chrome / Chromium files
  dist/            # ../src will be copied here
  mainfest.json
  ...
firefox-mv2/       # Firefox Manifest V2 files
  dist/            # ../src will be copied here
  mainfest.json
  background.html  # Loads ESModule
  ...
firefox/           # Firefox Manifest V3 files
  dist/            # ../src will be copied here
  mainfest.json
  background.html  # Loads ESModule
  ...
compile.sh         # Copies src/**/* to <platform>/dist/
```

### Install dependencies

```
npm install -g web-ext
npm install
```

### Debugging

Since the source code are copied to platform-specific folders by `compile.sh`, it is recommended to use the auto-reload test script.

```sh
npm debug-chrome
npm debug-firefox
npm debug-firefox-mv3   // Requires Firefox Developer Edition
```

For manual debugging without auto-reload:

- Chrome: [Window] Menu -> Extensions -> Load unpacked extension
- Firefox: [Tools] Menu -> Add-ons -> [Gear] Icon -> Debug Add-ons -> Load Temporary Add-on

#### Debug with Firefox XPI Package

To debug some behaviors such as Firefox restarts (for example, are context menus installed properly),
it is necessary to build an XPI package and install it on Firefox. Temporary Add-Ons won't be enough
because they get uninstalled after Firefox quits.

Firefox checks signature when installing XPI. To do so,

1. Grab [API keys](https://addons.mozilla.org/en-US/developers/addon/api/key/) from Firefox Add-On
2. Bump version in `manifest.json` and use something like `2.3.4b1` or `2.3.4rc1`
3. Run:
    ```shell
    web-ext sign --channel=unlisted --api-key=... --api-secret=...
    ```

It'll create an XPI that is signed with your Firefox Add-Ons account. The file will also be
uploaded to Add-On Developer Hub as unlisted.

Note that Firefox Add-On keeps track of all the versions that has ever been uploaded, including
'self-distributed' (`channel=unlisted`). That's why using a `b1` or `rc1` suffix is preferred.

See https://extensionworkshop.com/documentation/develop/getting-started-with-web-ext/

### Tests

Unit tests are written in mocha, `./test/**/*.test.js`.

To run, use `npm test`.

### QA

There is a [qa.html](./test/qa.html) that includes various edge test cases. Open it in the browser, then try Copy as Markdown with the content in it.

## License

See [MIT-LICENSE.txt](./MIT-LICENSE.txt)
