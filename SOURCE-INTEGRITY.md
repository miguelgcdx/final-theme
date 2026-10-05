# Source integrity

SHA-256 hashes identify the accepted bytes, not functional verification. The theme,
patch, and upstream license are unchanged copies. The four source pairs below are
recorded baseline and modified hashes; this export does not contain the full package.

## Exported copies

| File | SHA-256 |
|---|---|
| `themes/blue-theme-border-trial.json` | `76c419fe96381fd03a22e06c10499258014842031ef91699bb43f820d4314777` |
| `patches/gentle-pi-4.0.0-agents-skill-rows.patch` | `ee8abba3aa8d8b2d4ceaefabe06e32788a448869d8b9cf1737c78f3fa6288cb3` |
| `LICENSES/GENTLE-PI-MIT.txt` | `0457f5bcec3b3b211605dfb5d1a49042fd638f3686a410fe099c24a25af13c48` |

## Gentle Pi 4.0.0 patch boundary

Paths are relative to the official `gentle-pi@4.0.0` package root.
Check **all four originals before applying** the patch, then check all modified hashes.
Stop on any mismatch; do not adapt this patch blindly to another version.

| Source | Original SHA-256 | Modified SHA-256 |
|---|---|---|
| `lib/agents-protocol.ts` | `7472cd6e010ad6ac61d54ad13c0e45139b9e4004daa3b2a5e8a077689ddb89b9` | `4320542f7f2905af2295ac792b7906abd76849eeb140b99886bd34315918bf2d` |
| `lib/agents-runner.ts` | `255afd416be0f7e96dd112ea1569e3718da3548f716bc1229dc9537e14564a63` | `b65491c97c1e7e8b884ecbf66b1c9faf452f16bb0c0059f6dcad73c7849434f8` |
| `lib/agents-widget.ts` | `cfd37d3e56ee2d6fbba461bbbacc4e3e01c95efcf8a9b21dae0981548af9202a` | `d3343cbf8e35410d8f29a8f25f0070d5da183df87830436c3fbf2d537f281326` |
| `extensions/gentle-agents.ts` | `fd16d6545fe6aad31401adbea8265c2f931e4834ae7657fc2ff96cd879a36a19` | `8d3fbcff81448b5a9c9f2fa7e7998003303fec21cc64e812d4376c34fe6f925e` |

The modified widget hash is the accepted widget identity. Assignment labels are
prompt metadata, not proof that skills loaded or executed. Host API compatibility,
dependency resolution, setup commands, and runtime behavior remain unverified here.

## Attribution and notice

Upstream: [Gentleman-Programming/gentle-shell](https://github.com/Gentleman-Programming/gentle-shell),
`gentle-pi` version **4.0.0**, MIT. Its full local upstream notice is preserved in
[LICENSES/GENTLE-PI-MIT.txt](LICENSES/GENTLE-PI-MIT.txt), including the original
Copyright (c) 2025 Mario Zechner attribution. No separate new license is declared
for the user-authored theme or documentation; this notice is not a new root license grant.
