---
version: v1.4.8
category: API
redirect_from:
    - /docs/v0.24.0/api/crash-reporter/
    - /docs/v0.25.0/api/crash-reporter/
    - /docs/v0.26.0/api/crash-reporter/
    - /docs/v0.27.0/api/crash-reporter/
    - /docs/v0.28.0/api/crash-reporter/
    - /docs/v0.29.0/api/crash-reporter/
    - /docs/v0.30.0/api/crash-reporter/
    - /docs/v0.31.0/api/crash-reporter/
    - /docs/v0.32.0/api/crash-reporter/
    - /docs/v0.33.0/api/crash-reporter/
    - /docs/v0.34.0/api/crash-reporter/
    - /docs/v0.35.0/api/crash-reporter/
    - /docs/v0.36.0/api/crash-reporter/
    - /docs/v0.36.3/api/crash-reporter/
    - /docs/v0.36.4/api/crash-reporter/
    - /docs/v0.36.5/api/crash-reporter/
    - /docs/v0.36.6/api/crash-reporter/
    - /docs/v0.36.7/api/crash-reporter/
    - /docs/v0.36.8/api/crash-reporter/
    - /docs/v0.36.9/api/crash-reporter/
    - /docs/v0.36.10/api/crash-reporter/
    - /docs/v0.36.11/api/crash-reporter/
    - /docs/v0.37.0/api/crash-reporter/
    - /docs/v0.37.1/api/crash-reporter/
    - /docs/v0.37.2/api/crash-reporter/
    - /docs/v0.37.3/api/crash-reporter/
    - /docs/v0.37.4/api/crash-reporter/
    - /docs/v0.37.5/api/crash-reporter/
    - /docs/v0.37.6/api/crash-reporter/
    - /docs/v0.37.7/api/crash-reporter/
    - /docs/v0.37.8/api/crash-reporter/
    - /docs/latest/api/crash-reporter/
source_url: 'https://github.com/electron/electron/blob/master/docs/api/crash-reporter.md'
excerpt: "Submit crash reports to a remote server."
title: "crashReporter"
sort_title: "crashreporter"
---

# crashReporter

> Submit crash reports to a remote server.

Process: [Main](http://electron.atom.io/docs/tutorial/quick-start#main-process), [Renderer](http://electron.atom.io/docs/tutorial/quick-start#renderer-process)

The following is an example of automatically submitting a crash report to a
remote server:

```javascript
const {crashReporter} = require('electron')

crashReporter.start({
  productName: 'YourName',
  companyName: 'YourCompany',
  submitURL: 'https://your-domain.com/url-to-submit',
  autoSubmit: true
})
```

For setting up a server to accept and process crash reports, you can use
following projects:

* [socorro](https://github.com/mozilla/socorro)
* [mini-breakpad-server](https://github.com/electron/mini-breakpad-server)

Crash reports are saved locally in an application-specific temp directory folder.
For a `productName` of `YourName`, crash reports will be stored in a folder
named `YourName Crashes` inside the temp directory. You can customize this temp
directory location for your app by calling the `app.setPath('temp', '/my/custom/temp')`
API before starting the crash reporter.

## Methods

The `crashReporter` module has the following methods:

### `crashReporter.start(options)`

* `options` Object
  * `companyName` String (optional)
  * `submitURL` String - URL that crash reports will be sent to as POST.
  * `productName` String (optional) - Defaults to `app.getName()`.
  * `autoSubmit` Boolean (optional) - Send the crash report without user interaction.
    Default is `true`.
  * `ignoreSystemCrashHandler` Boolean (optional) - Default is `false`.
  * `extra` Object (optional) - An object you can define that will be sent along with the
    report. Only string properties are sent correctly, Nested objects are not
    supported.

You are required to call this method before using other `crashReporter`
APIs.

**Note:** On macOS, Electron uses a new `crashpad` client, which is different
from `breakpad` on Windows and Linux. To enable the crash collection feature,
you are required to call the `crashReporter.start` API to initialize `crashpad`
in the main process and in each renderer process from which you wish to collect
crash reports.

### `crashReporter.getLastCrashReport()`

Returns [`CrashReport`](http://electron.atom.io/docs/api/structures/crash-report):

Returns the date and ID of the last crash report. If no crash reports have been
sent or the crash reporter has not been started, `null` is returned.

### `crashReporter.getUploadedReports()`

Returns [`CrashReport[]`](http://electron.atom.io/docs/api/structures/crash-report):

Returns all uploaded crash reports. Each report contains the date and uploaded
ID.

## Crash Report Payload

The crash reporter will send the following data to the `submitURL` as
a `multipart/form-data` `POST`:

* `ver` String - The version of Electron.
* `platform` String - e.g. 'win32'.
* `process_type` String - e.g. 'renderer'.
* `guid` String - e.g. '5e1286fc-da97-479e-918b-6bfb0c3d1c72'
* `_version` String - The version in `package.json`.
* `_productName` String - The product name in the `crashReporter` `options`
  object.
* `prod` String - Name of the underlying product. In this case Electron.
* `_companyName` String - The company name in the `crashReporter` `options`
  object.
* `upload_file_minidump` File - The crash report in the format of `minidump`.
* All level one properties of the `extra` object in the `crashReporter`
  `options` object.
