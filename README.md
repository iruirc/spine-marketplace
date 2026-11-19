# Spine marketplace

[![release](https://img.shields.io/github/v/tag/iruirc/spine-marketplace?sort=semver&label=release&color=0969da)](https://github.com/iruirc/spine-marketplace)
[![license](https://img.shields.io/github/license/iruirc/spine-marketplace?color=555)](LICENSE)
[![spine-toolkit](https://img.shields.io/github/v/tag/iruirc/spine-toolkit?sort=semver&label=spine-toolkit&color=0969da)](https://github.com/iruirc/spine-toolkit)
[![spine-platform-swift](https://img.shields.io/github/v/tag/iruirc/spine-platform-swift?sort=semver&label=spine-platform-swift&color=0969da)](https://github.com/iruirc/spine-platform-swift)
[![spine-platform-kotlin](https://img.shields.io/github/v/tag/iruirc/spine-platform-kotlin?sort=semver&label=spine-platform-kotlin&color=0969da)](https://github.com/iruirc/spine-platform-kotlin)
[![spine-driver-mobile](https://img.shields.io/github/v/tag/iruirc/spine-driver-mobile?sort=semver&label=spine-driver-mobile&color=0969da)](https://github.com/iruirc/spine-driver-mobile)
[![spine-driver-agent-device](https://img.shields.io/github/v/tag/iruirc/spine-driver-agent-device?sort=semver&label=spine-driver-agent-device&color=0969da)](https://github.com/iruirc/spine-driver-agent-device)

Five Claude Code plugins in three categories — the orchestrator, the platforms that teach it a stack,
and the drivers that let it drive a running app. All five install in Codex CLI too:

- **[spine-toolkit](https://github.com/iruirc/spine-toolkit)** — a task lifecycle orchestrator that
  knows no programming language. Install it alone and you get the process.
- **[spine-platform-swift](https://github.com/iruirc/spine-platform-swift)** — Swift and Apple knowledge, nine
  agents, and the manifest that tells `spine-toolkit` who to dispatch to. Declares `spine-toolkit`
  a dependency, so installing it installs both.
- **[spine-platform-kotlin](https://github.com/iruirc/spine-platform-kotlin)** — Kotlin knowledge for Android,
  Compose Desktop, JVM servers and KMP: sixteen agents fanned out by target, twenty-nine skills,
  and the manifest that tells `spine-toolkit` who to dispatch to. Declares `spine-toolkit` a
  dependency, so installing it installs both.
- **[spine-driver-mobile](https://github.com/iruirc/spine-driver-mobile)** — declares what the
  `mcp-devices` MCP server can drive on each surface, so Validation drives the app instead of
  handing every UI check to a human. An adapter: it does not ship or install the server.
- **[spine-driver-agent-device](https://github.com/iruirc/spine-driver-agent-device)** — the same
  for `callstack/agent-device`. Pick whichever server you already run; a project names one in its
  `## Validation` block.

## Claude Code

Add the marketplace, then install one platform: `spine-platform-swift` for a Swift project,
`spine-platform-kotlin` for a Kotlin one; either pulls `spine-toolkit` with it. For any other stack
install `spine-toolkit` alone and pair it with a platform plugin of your own. `spine-driver-mobile`
is optional: it lets Validation drive a simulator or device. Each line is a command of its own, with
nothing after the plugin name.

```text
/plugin marketplace add iruirc/spine-marketplace
/plugin install spine-platform-swift
/plugin install spine-platform-kotlin
/plugin install spine-toolkit
/plugin install spine-driver-mobile
```

## Codex

Add this repository as a Git-backed marketplace, then install the orchestrator, a platform and,
optionally, a driver. For a Kotlin project replace the third line with
`codex plugin add spine-platform-kotlin@spine`; the last line, the driver, is optional. The block
carries no comments on purpose: zsh, the macOS default, passes a pasted `#` and the words after it
on as arguments.

```bash
codex plugin marketplace add iruirc/spine-marketplace
codex plugin add spine-toolkit@spine
codex plugin add spine-platform-swift@spine
codex plugin add spine-driver-mobile@spine
```

Start a new Codex session after installation, then run `$spine-toolkit:setup` in the
project — [spine-toolkit's README](https://github.com/iruirc/spine-toolkit#codex-cli) says what it
sets up and what differs from Claude Code.

## Updating

Run these in a terminal, not inside a session. The blocks carry no comments on purpose: zsh, the
macOS default, does not read `#` as a comment when you paste into it, and passes the words on as
arguments.

Claude Code — refresh the marketplace, update every Spine plugin (keep only the lines for the ones
you installed; one already at its latest version stays as it is), then list the installed versions
and the marketplace release the update read. `claude plugin list` shows plugins only; the last line
prints the release commit of the local marketplace clone, `chore: release <version>`:

```bash
claude plugin marketplace update spine
claude plugin update spine-toolkit@spine
claude plugin update spine-platform-swift@spine
claude plugin update spine-platform-kotlin@spine
claude plugin update spine-driver-mobile@spine
claude plugin update spine-driver-agent-device@spine
claude plugin list | grep -A1 '@spine'
git -C ~/.claude/plugins/marketplaces/spine log -1 --oneline
```

Restart Claude Code: an update applies to sessions started after it.

Codex — refreshing the marketplace updates the installed plugins too; the second line lists the
versions now installed, the third prints the release commit of the marketplace clone, at the path
the upgrade reports as `Installed marketplace root`:

```bash
codex plugin marketplace upgrade spine
ls ~/.codex/plugins/cache/spine/*
git -C ~/.codex/.tmp/marketplaces/spine log -1 --oneline
```

A plugin whose new version is missing from that list is reinstalled with
`codex plugin add <plugin>@spine`, for example `codex plugin add spine-toolkit@spine`.

Start a new Codex session. Codex fingerprints a plugin's hooks by the whole plugin, so after every
update it asks to trust Spine's hooks again although they did not change: trust them (`t`).

Then read the release notes of each plugin you updated — the body of the `chore: release <version>`
commit behind each tag in its own repository:
[spine-toolkit](https://github.com/iruirc/spine-toolkit/tags),
[spine-platform-swift](https://github.com/iruirc/spine-platform-swift/tags),
[spine-platform-kotlin](https://github.com/iruirc/spine-platform-kotlin/tags),
[spine-driver-mobile](https://github.com/iruirc/spine-driver-mobile/tags),
[spine-driver-agent-device](https://github.com/iruirc/spine-driver-agent-device/tags). When a release
changes what a project config means, they say so first and name what to run; usually that is
`/setup` in Claude Code or `$spine-toolkit:setup` in Codex, once per project. It asks before it
changes anything.

Writing a platform plugin for another stack is a documented contract:
[`conventions/platform-contract.md`](https://github.com/iruirc/spine-toolkit/blob/main/conventions/platform-contract.md).
So is writing a driver for an MCP server you do not own:
[`conventions/driver-contract.md`](https://github.com/iruirc/spine-toolkit/blob/main/conventions/driver-contract.md).
