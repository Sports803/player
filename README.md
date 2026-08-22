# player

This is a static live-stream player. Pass one or more sources through the `mora` query parameter. The value must be URL-encoded when it contains its own query string.

## DASH with ClearKey

Direct DASH manifests are detected from `.mpd` URLs. ClearKey credentials can be provided either with the existing `drmkey=<keyId>:<key>` page parameter or alongside the MPD as `drmScheme=clearkey&drmLicense=<keyId>:<key>`. The player preserves the full MPD URL, including token and DRM query parameters, for the manifest request and passes the normalized 16-byte key pair to the Shaka and dash.js EME adapters.

Example source shape:

```text
index.html?mora=https%3A%2F%2Fexample.test%2Fmanifest.mpd%3F%7CdrmScheme%3Dclearkey%26drmLicense%3D<keyId>%3A<key>
```

The supplied StarHub link follows this format. Availability still depends on the origin accepting the token and the browser supporting Encrypted Media Extensions with ClearKey.
