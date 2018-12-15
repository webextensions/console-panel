# console-panel

A console panel within web page to help in the following use-cases:
* Get notification on console messages
* Console logging on mobile and tablet devices
* Console logging on Microsoft Edge / Internet Explorer (without opening native Developer Tools)

# Demo

[Click here](https://raw.githack.com/webextensions/console-panel/master/demo/demo.html) to view online demo.

# How to use

## Files to include

* [console-panel.css](src/console-panel.css)
* [console-panel.js](src/console-panel.js)

## Minimal setup

In your HTML file, add the following tags:
```html
<link rel="stylesheet" href="console-panel.css" />
<script src="console-panel.js"></script>
<script>
    consolePanel.enable();
</script>
```

## Full-featured setup

In your HTML file, add the following tags:
```html
<!-- Optional: Required for resizing panel height -->
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
<script src="https://cdn.jsdelivr.net/gh/RickStrahl/jquery-resizable@0.32/dist/jquery-resizable.min.js"></script>

<!-- Optional: Required for logging arrays and objects -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/native-promise-only/0.8.1/npo.js"></script> <!-- Optional: A polyfill for native ES6 Promises (Not required for modern browsers) -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/jsoneditor/5.26.2/jsoneditor.min.css" />
<script src="https://cdnjs.cloudflare.com/ajax/libs/jsoneditor/5.26.2/jsoneditor-minimalist.min.js"></script>

<!-- Optional: Required for logging DOM elements with syntax highlighting -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/prism/1.15.0/themes/prism.min.css" />
<script src="https://cdnjs.cloudflare.com/ajax/libs/prism/1.15.0/prism.min.js" data-manual></script>

<!-- Required: Code for console-panel -->
<link rel="stylesheet" href="console-panel.css" />
<script src="console-panel.js"></script>
<script>
    consolePanel.enable();
</script>
```

# API

## consolePanel.enable(config)

This will initiate the interception logic based on the configuration options and activate console-panel as requested.

### Configuration options

<details>
  <summary>position</summary>
  <p>

  **Summary**: Position of console-panel's icon  
  **Type**: `string`  
  **Supported positions**: `"top-left"`, `"top-right"`, `"bottom-left"`, `"bottom-right"`  
  **Default value**: `"bottom-right"`  
  **Example value**: `"top-right"`  

  </p>
</details>

<details>
  <summary>functionsToIntercept</summary>
  <p>

  **Summary**: List of console functions which should be intercepted  
  **Type**: `<falsy-value>` OR `string "all"` OR `array (of strings)`  
  **Supported function names**: `"window.onerror"`, `"console.error"`, `"console.warn"`, `"console.info"`, `"console.log"`  
  **Default value**: `"all"`,  
  **Example value**: `["window.onerror", "console.error"]`  
  **Notes**: `console.clear()` would always get intercepted when `consolePanel.enable(config)` is called  

  </p>
</details>

<details>
  <summary>showOnlyForTheseRelevantMessages</summary>
  <p>

  **Summary**: List of console function calls for which console-panel icon should be shown  
  **Type**: `<falsy-value>` OR `string "all"` OR `array (of strings)`  
  **Supported function names**: `"window.onerror"`, `"console.error"`, `"console.warn"`, `"console.info"`, `"console.log"`  
  **Default value**: `null`  
  **Example value**: `["window.onerror", "console.error", "console.warn"]`  
  **Notes**: If it is a `<falsy-value>`, then console-panel notification icon would be shown all the time  

  </p>
</details>

<details>
  <summary>strongNotificationFor</summary>
  <p>

  **Summary**: List of console function calls for which console-panel notification should be shown strongly  
  **Type**: `<falsy-value>` OR `array (of strings)`  
  **Supported function names**: `"window.onerror"`, `"console.error"`, `"console.warn"`, `"console.info"`, `"console.log"`  
  **Default value**: `["window.onerror", "console.error"]`  
  **Example value**: `["window.onerror", "console.error", "console.warn"]`  

  </p>
</details>

<details>
  <summary>skipStrongNotificationIfNoStackTrace</summary>
  <p>

  **Summary**: When it is set as true, "strong-notification" effect is not shown for errors for which stack trace is not available. This can be used to avoid highlighting errors which are occurring due to a cross-origin / third-party script.  
  **Type**: `boolean`  
  **Allowed values**: `<falsy-value>` OR `<truthy-value>`  
  **Default value**: `false`  
  **Example value**: `false`  

  </p>
</details>

<details>
  <summary>reportLogLines</summary>
  <p>

  **Summary**: When it is set as `true`, the corresponding code line is mentioned along with each console entry. When it is set as `true`, it may interrupt your debugging session if you are using the "Pause on caught exceptions" feature in browser DevTools  
  **Type**: `boolean`  
  **Allowed values**: `<falsy-value>` OR `<truthy-value>`  
  **Default value**: `true`  
  **Example value**: `true`  

  </p>
</details>

<details>
  <summary>doNotUseWhenDevToolsMightBeOpenInTab</summary>
  <p>

  **Summary**: Disable console-panel if browser DevTools might be open within the tab  
  **Type**: `boolean`  
  **Allowed values**: `<falsy-value>` OR `<truthy-value>`  
  **Default value**: `false`  
  **Example value**: `false`  
  **Reference**: https://github.com/sindresorhus/devtools-detect#support  

  </p>
</details>

<details>
  <summary>disableButtonTitle</summary>
  <p>

  **Summary**: Customize the title for the "disable" button in console-panel  
  **Type**: `string`  
  **Allowed values**: Any non-empty string  
  **Default value**: `"Disable for this instance"`  
  **Example value**: `"Disable\n(and keep disabled)"`  

  </p>
</details>

<details>
  <summary>beforeDisableButtonClick</summary>
  <p>

  **Summary**: Function to be called before performing the default action for "disable" button  
  **Type**: `function`  
  **Example value**: `function () { localStorage['console-panel-status'] = 'disabled'; }`  
  **Notes**: If this function returns boolean `false`, then the default action would not be performed  

  </p>
</details>

### Examples

```js
// Enable console-panel in default mode.
// Use this configuration for getting notified about console messages and
// debugging for mobile devices and IE/Edge.
consolePanel.enable();
```

```js
// Intercept only error logs.
// Use this configuration to ensure that you don't miss out on any errors, while
// continuing to use native DevTools logger for all other log entries.
consolePanel.enable({
    functionsToIntercept: ['window.onerror', 'console.error']
});
```

```js
// Do not use console-panel when browser's DevTools might be open in the tab.
// Use this configuration to avoid using console-panel when you are debugging
// with the help of native DevTools.
consolePanel.enable({
    doNotUseWhenDevToolsMightBeOpenInTab: true
});
```

```js
// Hide console-panel icon by default. But, show it as soon as any console
// related message shows up.
// Use this configuration if you wish to avoid seeing the console-panel icon as
// long as it doesn't serve an important purpose.
consolePanel.enable({
    showOnlyForTheseRelevantMessages: 'all'
});
```

```js
// Show strong notification for all console related messages.
// Use this configuration if you wish to catch any console messages missed out
// from code clean up.
consolePanel.enable({
    strongNotificationFor: 'all'
});
```

```js
// Do not report the code lines for messages in console-panel.
// Use this configuration if you wish to use "Pause on caught exceptions"
// feature in browser DevTools (which would otherwise get interrupted by dummy
// errors thrown by console-panel to report the code lines accurately).
consolePanel.enable({
    reportLogLines: false
});
```

```js
// Skip strong-notification effect for errors without stack trace.
// Use this configuration if you wish to ignore errors caused by cross-origin
// (usually third-party) scripts where stack traces are not available.
consolePanel.enable({
    skipStrongNotificationIfNoStackTrace: true
});
```

```js
// Add custom logic around the "Disable for this instance" button.
// Use this configuration if you wish to disable console-panel and keep it
// disabled for future page loads.
if (localStorage['console-panel-status'] !== 'disabled') {
    consolePanel.enable({
        disableButtonTitle: 'Disable\n(and keep disabled)',
        beforeDisableButtonClick: function () {
            localStorage['console-panel-status'] = 'disabled';
        }
    });
}
```

## consolePanel.disable()

If you wish to deactivate console-panel, run `consolePanel.disable()`. It will restore the intercepted functions to their original state.

# Browser support

<!--
https://stackoverflow.com/questions/13808020/include-an-svg-hosted-on-github-in-markdown/16462143#16462143
-->

<img width="32" alt="Google Chrome"   src="images/logo-google-chrome.svg?sanitize=true" > Google Chrome

<img width="32" alt="Microsoft Edge"  src="images/logo-microsoft-edge.svg?sanitize=true"> Microsoft Edge (and Internet Explorer)

<img width="32" alt="Mozilla Firefox" src="images/logo-firefox_edited.png"              > Mozilla Firefox

<img width="32" alt="Opera"           src="images/logo-opera.svg?sanitize=true"         > Opera

# Limitations / notable behavior

* `console.clear()` is always intercepted whenever `consolePanel.enable()` is called.
* In Microsoft Edge (EdgeHTML) and Internet Explorer, the console functions are
  not intercepted if the browser's native Developer Tools are in activated state
  (`window.onerror` can still be intercepted). Also, for these browsers, if the
  native Developer Tools have been opened once, then the intercepted calls to the
  console logging function calls are absorbed (rather than intercepted).
* During a page load, if `consolePanel.enable(config)` has already been called once,
  then calling `consolePanel.enable(config_new)` with a different config may not work
  well for all the cases.

# TODO

[TODO.md](TODO.md)

# Alternative solutions you may like

<details>
  <summary>Remote debugging</summary>
  <p>

  * Android: https://developers.google.com/web/tools/chrome-devtools/remote-debugging/
  * iOS: https://github.com/google/ios-webkit-debug-proxy

  </p>
</details>

<details>
  <summary>vConsole</summary>
  <p>

  A lightweight, extendable front-end developer tool for mobile web page (https://github.com/Tencent/vConsole)

  </p>
</details>

<details>
  <summary>Firebug Lite</summary>
  <p>

  In your HTML file, add the following tags:
  ```html
  <script src="https://getfirebug.com/firebug-lite.js"></script>
  <script>
      firebug.init();
  </script>
  ```

  References:
  * https://blog.getfirebug.com/2013/08/21/firebug-1-12-0/
  * https://blog.getfirebug.com/2013/05/02/future-of-firebug-lite/
  * http://www.softwareishard.com/blog/planet-mozilla/how-to-start-with-firebug-lite/

  </p>
</details>

# About this project

## Author

* Priyank Parashar - [GitHub](https://github.com/paras20xx) | [Twitter](https://twitter.com/paras20xx) | [LinkedIn](https://linkedin.com/in/ParasharPriyank/)

## Connect to us

* https://webextensions.org/
* [GitHub](https://github.com/webextensions/live-css-editor)
* [Twitter](https://twitter.com/webextensions)

## License

* [MIT](LICENSE)
