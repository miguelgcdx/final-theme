# final-theme

A lean Pi color theme plus a version-locked Gentle Agents skill-row patch.
The runtime theme name stays **`blue-theme-border-trial`**; `final-theme` is only
the repository name. Accepted theme/patch bytes are preserved, including the palette
and compact card's no-blue-border treatment. Terminal fonts are not controlled by
Pi themes, and no font settings or personal CLI configuration are included.

**Manual recipe: UNTESTED.** These setup commands were not run for this export.
Runtime behavior and new-owner activation have not been functionally verified.
Repository: https://github.com/miguelgcdx/final-theme.

## Prerequisites

- Pi **1.0.3**, Node **22.19.0+**, npm, tar, patch, and SHA-256 tools.
- A full official **`gentle-pi@4.0.0`** package, not four loose patched files.
- Gentle card style **FLOAT** (the default) for compact float cards. A saved
  **neon** style does not activate the compact float branch. The fragment below
  does not change card style; no guessed style key or command is supplied.

Review extension code before loading it: extensions execute with your permissions.
The patch adds bounded skill-assignment labels, not proof of skill execution.

## Manual setup

Run these steps only after reviewing them. Stop on any existing target, command
failure, or hash mismatch; do not overwrite an existing setup. Retain your current
settings separately before a manual merge. No installer is included.

### 1. Obtain the export and install the theme

Obtain these files from the repository. Set `EXPORT` to
its absolute local path. Compare theme and patch hashes with [SOURCE-INTEGRITY.md](SOURCE-INTEGRITY.md).
The destination uses the global agent directory, regardless of your current directory.

```sh
set -eu
EXPORT="/absolute/path/to/final-theme"
sha256sum "$EXPORT/themes/blue-theme-border-trial.json" \
  "$EXPORT/patches/gentle-pi-4.0.0-agents-skill-rows.patch"
mkdir -p "$HOME/.pi/agent/themes"
# Exclusive file creation: an existing file is an error, not a replacement.
(set -C; cat "$EXPORT/themes/blue-theme-border-trial.json" > \
  "$HOME/.pi/agent/themes/blue-theme-border-trial.json")
```

### 2. Obtain and materialize the full official package and dependencies

Keep the complete tarball contents: imports, runtime modules, assets, and child
helpers are part of the closure. The patch alone is **not** a complete Agents extension.
Create fresh, user-owned global folders; retain the download for provenance.

```sh
OWNER="$HOME/.pi/agent/packages/blue-theme-border-trial-gentle-pi"
DOWNLOAD="$HOME/.pi/agent/packages/blue-theme-border-trial-download"
for target in "$OWNER" "$DOWNLOAD"; do
  if [ -e "$target" ] || [ -L "$target" ]; then
    printf 'STOP: target already exists: %s\n' "$target" >&2
    exit 1
  fi
done
mkdir -p "$HOME/.pi/agent/packages"
mkdir "$OWNER" "$DOWNLOAD"
npm pack gentle-pi@4.0.0 --ignore-scripts --pack-destination "$DOWNLOAD"
tar -xzf "$DOWNLOAD/gentle-pi-4.0.0.tgz" -C "$OWNER" --strip-components=1
npm install --prefix "$OWNER" --ignore-scripts --omit=dev \
  --legacy-peer-deps --no-audit --no-fund
```

Pi 1.0.3 supplies the host Pi peers and `typebox`; do not bundle duplicate host
packages. `--legacy-peer-deps` avoids npm installing those peers. The local package
still needs its non-host dependency root **`@heyhuynhgiabuu/pi-pretty@0.6.27`**
and its transitive dependencies. The install uses the official package manifest,
omits dev dependencies, and disables lifecycle scripts. Future transitive resolution
is not guaranteed identical; this export includes neither dependencies nor a lockfile.

### 3. Guard all four originals, then apply the version-locked patch

From the materialized package root, require all four original hashes to match.
Never apply this patch blindly to future Gentle versions.

```sh
cd "$OWNER"
sha256sum -c <<'SHA256'
7472cd6e010ad6ac61d54ad13c0e45139b9e4004daa3b2a5e8a077689ddb89b9  lib/agents-protocol.ts
255afd416be0f7e96dd112ea1569e3718da3548f716bc1229dc9537e14564a63  lib/agents-runner.ts
cfd37d3e56ee2d6fbba461bbbacc4e3e01c95efcf8a9b21dae0981548af9202a  lib/agents-widget.ts
fd16d6545fe6aad31401adbea8265c2f931e4834ae7657fc2ff96cd879a36a19  extensions/gentle-agents.ts
SHA256
patch --batch --forward -p1 < "$EXPORT/patches/gentle-pi-4.0.0-agents-skill-rows.patch"
sha256sum lib/agents-protocol.ts lib/agents-runner.ts \
  lib/agents-widget.ts extensions/gentle-agents.ts
```

Compare **all four modified hashes** with SOURCE-INTEGRITY.md before continuing;
the accepted widget hash is `d3343cbf8e35410d8f29a8f25f0070d5da183df87830436c3fbf2d537f281326`.
Stop on any mismatch; successful patch application alone is not sufficient.

### 4. Merge the owner filter into global settings

Manually **merge** [settings/agents-owner.fragment.json](settings/agents-owner.fragment.json)
into **`~/.pi/agent/settings.json`**. Do **not** overwrite the full settings file.
Preserve every unrelated package and field. Reconcile an existing Gentle npm entry
rather than adding a duplicate, preserving any additional intentional filters.

The npm Gentle entry excludes only `extensions/gentle-agents.ts`, leaving other
Gentle resources available. The local entry enables only that Agents extension and
no local themes, prompts, or skills. Its relative path resolves from
`~/.pi/agent/`, **not the launch directory**. Review other explicit extension paths
for duplicate Agents owners. The unpinned npm entry allows other Gentle components
to update; it does not update the local owner.

### 5. Fully restart normal Pi and inspect manually

Exit Pi fully and start a new normal interactive Pi session, not a child/RPC session.
Do not rely on `/reload` to replace installed/imported package modules. Confirm the
selected theme, single Agents owner, FLOAT prerequisite, and assigned-skill rows
manually. Activation and these functional checks remain human-pending.

## Boundaries and maintenance

Local Agents tools and behavior now require deliberate manual upgrades. Host or
Gentle API changes may break compatibility even while hashes remain unchanged.
Project settings, deliberate CLI overrides, and other extensions can override the
global theme/resources; this export does not reset them or force selection.

No personal settings, backups, sessions, installed dependencies, full package source,
screenshots, native-review artifacts, or private work history are distributed.

## Upstream notice

Patch attribution: **Gentleman-Programming/gentle-shell**, **gentle-pi 4.0.0**, MIT.
The complete upstream MIT notice is copied unchanged to
[LICENSES/GENTLE-PI-MIT.txt](LICENSES/GENTLE-PI-MIT.txt).
No separately declared new license is provided for the user-authored theme or docs;
there is no new root license grant. See SOURCE-INTEGRITY.md for copied-byte provenance.
