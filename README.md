# Final Fantasy Tactics decompilation

> ## About this fork
>
> This is [GhostWriter42](https://github.com/GhostWriter42)'s fork of [adamrt/fft_decomp](https://github.com/adamrt/fft_decomp). **The decompilation itself is upstream's work and is unchanged here.**
> This default branch (`fork`) is upstream's `master` (as of 2026-10-03, commit `2b09e33`) plus this notice and nothing else. The fork's `master` is kept as a pure mirror of upstream.
>
> What this fork adds, as separate branches that are each based on upstream's `master` (so each could be proposed upstream on its own). All three keep the build byte-exact:
> `make validate` (here `fft.ps1 validate`) reports all 5,296 functions matching, and `fmt-check` and `check-config` pass on top of `2b09e33`.
>
> | Branch | Commits | What it changes |
> |---|---|---|
> | [`remove-unneeded-pins-and-barriers`](../../tree/remove-unneeded-pins-and-barriers) | 6 | Removes **172 compiler-workaround constructs** that no longer affect the output: 9 register pins, 2 empty asm barriers, 11 volatile reload casts, 9 volatile pointer views and 141 workarounds in the reconstructed PS1 SDK (132 + 9, in two commits). Found by removing each one and checking the function still matches. |
> | [`windows-line-endings`](../../tree/windows-line-endings) | 2 | Adds a `.gitattributes` that keeps the checkout on LF and a README note on the Windows CRLF pitfall (a CRLF checkout makes `check-config` fail with "not in canonical form"). |
> | [`document-native-port-ub`](../../tree/document-native-port-ub) | 2 | `QUIRKS.md` only: undefined behaviour and retail-ABI accidents that a native (non-PS1) build has to neutralise, including a null test after a dereference that modern compilers delete. Items upstream has since documented were dropped. |
>
> No pull request has been opened yet. The upstream maintainer has independently fixed several of the same retail-register problems this fork's native-port work ran into; where they did, the fork's version was dropped.
>
> **What this is used for:** a native build of the game that is checked, frame by frame, against the original machine code (RAM, SDK calls and VRAM), with a GPU renderer, sound and a two-player prototype. All of that lives in a separate repository,
> **[GhostWriter42/fft-native-lab](https://github.com/GhostWriter42/fft-native-lab)** (it never edits the decomp; it builds the decomp's C with a few reviewed text patches applied to a generated copy). Its README states, in detail, how it differs from this repository.
>
> The build instructions below are upstream's and work unchanged. Game files are not included, here or in the lab repository; you need your own copy of the game.

> Status: 100% decompiled and byte-exact ✅

A matching decompilation of the North American PlayStation release of *Final
Fantasy Tactics* (`SCUS-94221` [redump](http://redump.org/disc/55/)). Every game
function is C that compiles to the original bytes, and the rebuilt disc is a
byte-for-byte match.

Game files and proprietary Sony tools are not included.

## Next Steps

The game is 100% completely decompiled, but there are plenty of semantic changes
and cleanup to be done. Variable and function names, correcting types, improving
enum usage/naming, etc.

Each time a change is made `make validate` will ensure the changes still match
byte-for-byte.

## Getting started

Requirements
- Docker
- `SCUS-94221` BIN at `./scus-94221.bin` (optional)

Then run:

```sh
# Prepare environment
make bootstrap

# Then use one of the following (make help for more):
make validate  # verify all code against checksums (without BIN)
make build     # build every module and a byte-matching disc (needs the BIN)
```

`make build` writes `build/disc/output-scus-94221.bin` and `.cue`.

## Thanks

This project is an ode to the [FFHacktics](https://ffhacktics.com/wiki/)
community. This project would not be possible without it. ❤️
