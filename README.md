<p align="center">
  <img src="assets/icons/app/linux/exports/hicolor/256x256/apps/io.tangencylabs.solidcraft.png" width="96" height="96" alt="SolidCraft" />
</p>

<h1 align="center">MeshCraft</h1>

<p align="center">
  <strong>An open-source parametric 3D modeling application built in pure Rust.</strong><br />
  Sketch, constrain, and build solids with a parametric feature timeline. Change a dimension
  and watch your model rebuild. Run it on your desktop, in your browser, or let AI agents
  model through the built-in MCP server.
</p>

## Download

- **Windows:** the [installer][windows-msi].
- **macOS:** the [disk image][macos-dmg] (Apple silicon and Intel).
- **Linux:** the [AppImage][linux-appimage], or `sudo snap install solidcraft` ([Snap Store][snap-store]).
- **Web:** [try it in your browser][web-app], no install.

ARM64 and 32-bit builds, portable zip, .deb, .rpm, Flatpak and the other packages are on the
[downloads page](docs/downloads.md). The macOS app is signed with a Developer ID and notarized by
Apple. The Windows downloads aren't signed yet: choose **More info**, then **Run anyway** in
SmartScreen.

## Build from source

```sh
cargo run --release -p solidcraft -- --sample      # desktop app with a sample part
cargo run --release -p solidcraft-cli -- commands  # the command registry
cargo run --release -p solidcraft-cli -- run examples/bracket.json --out bracket.step
cargo xtask ci                                     # fmt, clippy, tests, icons, assets, layers, wasm

rustup target add wasm32-unknown-unknown
# cd apps/solidcraft-web && trunk serve --release --no-autoreload
cd apps/solidcraft-web && trunk serve

docker compose -f packaging/docker/compose.yaml up -d --build   # build the image from your checkout instead
docker compose -f packaging/docker/compose.yaml up -d      # start

# aarch64, x86_64
packaging/macos/package.sh --build-only x86_64

dotnet tool install --global wix --version 5.0.2
wix extension add -g WixToolset.UI.wixext/5.0.2 WixToolset.Util.wixext/5.0.2
# arm64, x64, x86
./packaging/windows/package.ps1 -Arch x64
```

More commands, the Windows cross-build and the browser build are in [docs/building.md](docs/building.md).

## AI agents (MCP)

`solidcraft-cli mcp` is a [Model Context Protocol](https://modelcontextprotocol.io) server. Headless it
runs its own design session, or with `--connect` it drives the running app so you can watch.
Claude Code: `claude mcp add solidcraft -- solidcraft-cli mcp`. If a terminal doesn't find
`solidcraft-cli` (older Windows installers don't put it on `PATH`), give its full path (by default
`"C:\\Program Files\\SolidCraft\\solidcraft-cli.exe"`). Tools, access rules and an example session:
[docs/mcp.md](docs/mcp.md).

## Scripts

A script is a JSON list of the same commands the UI runs, executed with `solidcraft-cli run`.
Format and example: [docs/scripts.md](docs/scripts.md). Pages can also be driven through the
[browser command API](docs/browser-api.md).

## Contributing

Help is welcome. [CONTRIBUTING.md](CONTRIBUTING.md) covers how to build and test, the code map,
the clean-room and licence rules, and where help is wanted most. 

[windows-msi]: https://github.com/tangency-labs/solidcraft/releases/latest/download/solidcraft-windows-x64.msi
[macos-dmg]: https://github.com/tangency-labs/solidcraft/releases/latest/download/solidcraft-macos-universal.dmg
[linux-appimage]: https://github.com/tangency-labs/solidcraft/releases/latest/download/solidcraft-linux-x86_64.AppImage
[snap-store]: https://snapcraft.io/solidcraft
[web-app]: https://solidcraft.io/app
