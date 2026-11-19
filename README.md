# Haiven CLI

`haiven` is a command-line tool for game developers working with the Haiven
SDK. It runs on Linux, macOS, and Windows. Use `--json` for a single structured
result with stable outcome and diagnostic codes.

## Install or build

### Linux and macOS

```sh
curl -fsSL https://raw.githubusercontent.com/haiven-dev/haiven-cli/v0.0.1/scripts/install.sh | sh
```

The installer puts `haiven` in `~/.local/bin` by default. If your shell
cannot find it, add that directory to your `PATH`.

### Windows

```powershell
irm https://raw.githubusercontent.com/haiven-dev/haiven-cli/v0.0.1/scripts/install.ps1 | iex
```

Open a new terminal to use the updated user `PATH`. Both installers check
the downloaded archive against the release's `checksums.txt`.

### From source

With the Go version in `go.mod`, build from this checkout with
`go build -trimpath -o dist/ ./cmd/haiven`. Run `haiven version` and
`haiven capabilities --json` with the executable you installed or built.

## Integrate an existing Unity project

Read the bundled guide with `haiven sdk starter describe unity-existing`.
Choose a release in the [Unity SDK repository][unity] and resolve it to its
full Git commit. Set `UNITY_SDK_REVISION` to that commit for the shell
examples. The manual guide works with Unity's Package Manager.

For optional command-line integration, install Python 3 and Git. Set
`HAIVEN_STARTER_RUNNER_DIR` to the absolute path of `starter-runners/unity-existing`
in this matching CLI source checkout. With the project already in Git, preview
the package edit before applying it.

```sh
haiven sdk starter integrate unity-existing --project ./MyGame \
  --param sdk_revision="$UNITY_SDK_REVISION" --preview
haiven sdk starter integrate unity-existing --project ./MyGame \
  --param sdk_revision="$UNITY_SDK_REVISION" --yes
```

Apply requires a clean Git worktree. After writing the pin, it reports
`starter-manual-required` and exits 3 while Editor import is pending. Follow the
steps in the result to import the package and wire your game code. Verify
with the native Editor and the bundled guide.

## Open a sample project

When a sample repository is available, clone it and check out the full commit
you want to run. Pass that checkout and commit to `haiven sdk demo open`.

- [Boss Room][bossroom] is a complete Unity project. Run `git lfs pull` in the
  checkout before opening it. Use `--unity` for the Unity Editor executable.
  Run `haiven sdk demo register unity-bossroom` with the same checkout and
  commit to add it to Unity Hub.
- [Lyra][lyra] supplies plugins and an overlay for your Epic Fab project.
  Apply the overlay as its README describes. Pass the overlay checkout and
  commit with `--sdk-root` and `--sdk-revision`, the assembled project with
  `--content`, and the Unreal installation with `--engine`.
- [Godot Demo][godot] includes its addon. Open it with the Godot Editor on PATH
  or pass the executable with `--godot`.
- [Unreal Chat][unreal-chat] is a complete Unreal project. Open its
  `HaivenChatStarter.uproject` directly in Unreal Editor.

For example, in a shell where `GODOT_DEMO_REVISION` contains a full reviewed commit:

```sh
haiven sdk demo open godot-demo --sdk-root ./GodotDemo --sdk-revision "$GODOT_DEMO_REVISION"
```

Use the engine's build or export tools for the standalone projects. The CLI
reports a checkout or revision mismatch before launching an editor.

## API descriptions and command schemas

Pass a published OpenAPI document with `haiven api list --openapi <file-or-url>`.
Use `haiven api call ... --dry-run` to validate and preview a request. The CLI
requires verified contract evidence before it sends a request. Start with
`haiven capabilities` and `haiven schema project` to inspect commands and project
version pins. The JSON schemas live in [schemas](schemas/).

The source license is [Apache 2.0](LICENSE). Release archives include the
[Haiven notice](NOTICE) and third-party dependency notices.

[unity]: https://github.com/haiven-dev/haiven-sdk-unity
[bossroom]: https://github.com/haiven-dev/haiven-sdk-unity-bossroom
[lyra]: https://github.com/haiven-dev/haiven-sdk-unreal-lyra
[godot]: https://github.com/haiven-dev/haiven-sdk-godot-demo
[unreal-chat]: https://github.com/haiven-dev/haiven-sdk-unreal-chat
