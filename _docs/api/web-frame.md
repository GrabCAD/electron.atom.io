---
version: v1.4.8
category: API
redirect_from:
    - /docs/v0.24.0/api/web-frame/
    - /docs/v0.25.0/api/web-frame/
    - /docs/v0.26.0/api/web-frame/
    - /docs/v0.27.0/api/web-frame/
    - /docs/v0.28.0/api/web-frame/
    - /docs/v0.29.0/api/web-frame/
    - /docs/v0.30.0/api/web-frame/
    - /docs/v0.31.0/api/web-frame/
    - /docs/v0.32.0/api/web-frame/
    - /docs/v0.33.0/api/web-frame/
    - /docs/v0.34.0/api/web-frame/
    - /docs/v0.35.0/api/web-frame/
    - /docs/v0.36.0/api/web-frame/
    - /docs/v0.36.3/api/web-frame/
    - /docs/v0.36.4/api/web-frame/
    - /docs/v0.36.5/api/web-frame/
    - /docs/v0.36.6/api/web-frame/
    - /docs/v0.36.7/api/web-frame/
    - /docs/v0.36.8/api/web-frame/
    - /docs/v0.36.9/api/web-frame/
    - /docs/v0.36.10/api/web-frame/
    - /docs/v0.36.11/api/web-frame/
    - /docs/v0.37.0/api/web-frame/
    - /docs/v0.37.1/api/web-frame/
    - /docs/v0.37.2/api/web-frame/
    - /docs/v0.37.3/api/web-frame/
    - /docs/v0.37.4/api/web-frame/
    - /docs/v0.37.5/api/web-frame/
    - /docs/v0.37.6/api/web-frame/
    - /docs/v0.37.7/api/web-frame/
    - /docs/v0.37.8/api/web-frame/
    - /docs/latest/api/web-frame/
source_url: 'https://github.com/electron/electron/blob/master/docs/api/web-frame.md'
excerpt: "Customize the rendering of the current web page."
title: "webFrame"
sort_title: "webframe"
---

# webFrame

> Customize the rendering of the current web page.

Process: [Renderer](http://electron.atom.io/docs/tutorial/quick-start#renderer-process)

An example of zooming current page to 200%.

```javascript
const {webFrame} = require('electron')

webFrame.setZoomFactor(2)
```

## Methods

The `webFrame` module has the following methods:

### `webFrame.setZoomFactor(factor)`

* `factor` Number - Zoom factor.

Changes the zoom factor to the specified factor. Zoom factor is
zoom percent divided by 100, so 300% = 3.0.

### `webFrame.getZoomFactor()`

Returns `Number` - The current zoom factor.

### `webFrame.setZoomLevel(level)`

* `level` Number - Zoom level

Changes the zoom level to the specified level. The original size is 0 and each
increment above or below represents zooming 20% larger or smaller to default
limits of 300% and 50% of original size, respectively.

### `webFrame.getZoomLevel()`

Returns `Number` - The current zoom level.

### `webFrame.setZoomLevelLimits(minimumLevel, maximumLevel)`

* `minimumLevel` Number
* `maximumLevel` Number

**Deprecated:** Call `setVisualZoomLevelLimits` instead to set the visual zoom
level limits. This method will be removed in Electron 2.0.

### `webFrame.setVisualZoomLevelLimits(minimumLevel, maximumLevel)`

* `minimumLevel` Number
* `maximumLevel` Number

Sets the maximum and minimum pinch-to-zoom level.

### `webFrame.setLayoutZoomLevelLimits(minimumLevel, maximumLevel)`

* `minimumLevel` Number
* `maximumLevel` Number

Sets the maximum and minimum layout-based (i.e. non-visual) zoom level.

### `webFrame.setSpellCheckProvider(language, autoCorrectWord, provider)`

* `language` String
* `autoCorrectWord` Boolean
* `provider` Object

Sets a provider for spell checking in input fields and text areas.

The `provider` must be an object that has a `spellCheck` method that returns
whether the word passed is correctly spelled.

An example of using [node-spellchecker][spellchecker] as provider:

```javascript
const {webFrame} = require('electron')
webFrame.setSpellCheckProvider('en-US', true, {
  spellCheck (text) {
    return !(require('spellchecker').isMisspelled(text))
  }
})
```

### `webFrame.registerURLSchemeAsSecure(scheme)`

* `scheme` String

Registers the `scheme` as secure scheme.

Secure schemes do not trigger mixed content warnings. For example, `https` and
`data` are secure schemes because they cannot be corrupted by active network
attackers.

### `webFrame.registerURLSchemeAsBypassingCSP(scheme)`

* `scheme` String

Resources will be loaded from this `scheme` regardless of the current page's
Content Security Policy.

### `webFrame.registerURLSchemeAsPrivileged(scheme[, options])`

* `scheme` String
* `options` Object (optional)
  * `secure` Boolean - (optional) Default true.
  * `bypassCSP` Boolean - (optional) Default true.
  * `allowServiceWorkers` Boolean - (optional) Default true.
  * `supportFetchAPI` Boolean - (optional) Default true.
  * `corsEnabled` Boolean - (optional) Default true.

Registers the `scheme` as secure, bypasses content security policy for resources,
allows registering ServiceWorker and supports fetch API.

Specify an option with the value of `false` to omit it from the registration.
An example of registering a privileged scheme, without bypassing Content Security Policy:

```javascript
const {webFrame} = require('electron')
webFrame.registerURLSchemeAsPrivileged('foo', { bypassCSP: false })
```

### `webFrame.insertText(text)`

* `text` String

Inserts `text` to the focused element.

### `webFrame.executeJavaScript(code[, userGesture])`

* `code` String
* `userGesture` Boolean (optional) - Default is `false`.

Evaluates `code` in page.

In the browser window some HTML APIs like `requestFullScreen` can only be
invoked by a gesture from the user. Setting `userGesture` to `true` will remove
this limitation.

### `webFrame.getResourceUsage()`

Returns `Object`:

* `images` [MemoryUsageDetails](http://electron.atom.io/docs/api/structures/memory-usage-details)
* `cssStyleSheets` [MemoryUsageDetails](http://electron.atom.io/docs/api/structures/memory-usage-details)
* `xslStyleSheets` [MemoryUsageDetails](http://electron.atom.io/docs/api/structures/memory-usage-details)
* `fonts` [MemoryUsageDetails](http://electron.atom.io/docs/api/structures/memory-usage-details)
* `other` [MemoryUsageDetails](http://electron.atom.io/docs/api/structures/memory-usage-details)

Returns an object describing usage information of Blink's internal memory
caches.

```javascript
const {webFrame} = require('electron')
console.log(webFrame.getResourceUsage())
```

This will generate:

```javascript
{
  images: {
    count: 22,
    size: 2549,
    liveSize: 2542,
    decodedSize: 478,
    purgedSize: 0,
    purgeableSize: 0
  },
  cssStyleSheets: { /* same with "images" */ },
  xslStyleSheets: { /* same with "images" */ },
  fonts: { /* same with "images" */ },
  other: { /* same with "images" */ }
}
```

### `webFrame.clearCache()`

Attempts to free memory that is no longer being used (like images from a
previous navigation).

Note that blindly calling this method probably makes Electron slower since it
will have to refill these emptied caches, you should only call it if an event
in your app has occurred that makes you think your page is actually using less
memory (i.e. you have navigated from a super heavy page to a mostly empty one,
and intend to stay there).

[spellchecker]: https://github.com/atom/node-spellchecker
