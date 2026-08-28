# player

This is a static live-stream player. Pass one or more sources through the `mora` query parameter. Values must be URL-encoded when they contain their own query string.

## Channel-page sources

The player also accepts a channel slug through `channel`, `source`, or `mora`. For example:

```text
https://sports803.github.io/player/?channel=tntsports1
https://sports803.github.io/player/?source=eurosport1
https://sports803.github.io/player/?mora=tntsports1
```

Channel slugs are restricted to letters, numbers, underscores, and hyphens and are resolved to `https://livetv.rkda.my.id/channels/[channel].html`. The page is fetched through the player's existing rotating CORS-proxy strategy. Its `DASH_URL`, `CLEARKEY_ID_HEX`, and `CLEARKEY_K_HEX` values are extracted from the HTML, validated as a 16-byte ClearKey pair, and loaded through the conditional Bitmovin adapter.

The existing player UI, controls, keyboard shortcuts, fullscreen handling, picture-in-picture behavior, health monitoring, analytics, and fallback flow remain in place. Bitmovin is loaded only for successfully scraped channel pages. If the page, manifest, credentials, or Bitmovin playback fails, the normal adapter chain and configured backup behavior are used instead.

## DASH with ClearKey

Direct DASH manifests are detected from `.mpd` URLs. ClearKey credentials can be provided either with the existing `drmkey=<keyId>:<key>` page parameter or alongside the MPD as `drmScheme=clearkey&drmLicense=<keyId>:<key>`. The player preserves the full MPD URL, including token and DRM query parameters, for the manifest request and passes the normalized 16-byte key pair to the Shaka and dash.js EME adapters.

Example source shape:

```text
index.html?mora=https%3A%2F%2Fexample.test%2Fmanifest.mpd%3F%7CdrmScheme%3Dclearkey%26drmLicense%3D<keyId>%3A<key>
```

The supplied StarHub link follows this format. Availability still depends on the origin accepting the token and the browser supporting Encrypted Media Extensions with ClearKey.

## Development

This repository is intentionally static and deploys directly to GitHub Pages. Because GitHub Pages does not provide a server runtime, channel-page retrieval uses the existing client-side proxy rotation rather than a dedicated backend endpoint. Proxy failures are logged for diagnostics without exposing ClearKey values in the interface.
