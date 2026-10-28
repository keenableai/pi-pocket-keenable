# Keenable for Pi Pocket

A drop-in module for [Pi Pocket](https://github.com/TannerMidd/pi-pocket) that gives Pi web search and page reading through [Keenable](https://keenable.ai). It works with no key.

- `keenable_search` searches the web. It gives the title, the address, and the start of the text of each page: 10 pages by default, 20 at most. It can keep to one `site`, and to pages published after or before a date (`published_after`, `published_before`, as `YYYY-MM-DD`).
- `keenable_fetch` reads one public page (http or https) as markdown: 20,000 characters by default. Keenable fetches the page, not your server.

Both tools only read. Plan mode lets them run, and a restart can run a call again.

## Install

- **From the gallery:** Menu → Extensions → **Discover**, find **Keenable web search**, install it, read the code, and turn it on.
- **By hand:** copy `keenable.ts` into the drop-in folder, `~/.pi-pocket/extensions/`, and turn it on in Menu → Extensions.

It needs Pi Pocket 0.22.0 or newer.

## An API key (optional)

Keyless calls have rate limits for each IP address. To raise them, put a Keenable API key in the module's settings, under the module in Menu → Extensions. The key is a secret setting: it stays on the server, and the module sends it only to `api.keenable.ai`.

## What it sends, and where

- Each call goes to `https://api.keenable.ai`, with the header `X-Keenable-Title: Pi Pocket`.
- `keenable_search` sends the query and its filters. `keenable_fetch` sends the page's address.
- With a key, calls go to `/v1/search` and `/v1/fetch`, with the key in `X-API-Key`. With no key, they go to `/v1/search/public` and `/v1/fetch/public`.
- An address on this machine or on a private network (`localhost`, `*.local`, `10.x`, `192.168.x`, and so on) is refused before any call.
- The module reads no other files, settings, or secrets, and sends no telemetry.

The text of the results is fenced with `host.text`, and Pi is told that it is data from the web, not instructions.

## Test

The tests run the module in a Pi Pocket on a temporary data folder, with a scripted model, as Pi Pocket's [Testing](https://github.com/TannerMidd/pi-pocket/blob/main/docs/extensions/testing.md) page tells.

```sh
git clone --branch v0.22.0 https://github.com/TannerMidd/pi-pocket ../pi-pocket
(cd ../pi-pocket && npm ci)

# Offline: requests to Keenable are stubbed.
POCKET_DIR=../pi-pocket npm test

# Also two keyless calls to the real API, through a Pi Pocket session.
KEENABLE_LIVE=1 POCKET_DIR=../pi-pocket npm test
```

The test links Pi Pocket's `node_modules` here, as Pi Pocket does in its drop-in folder.

## License

MIT. See [LICENSE](LICENSE).
