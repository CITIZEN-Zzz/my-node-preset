---
name: my-node-preset
description: Switch Node.js versions by meaning instead of memorizing numbers. Lets the user define old, stable, and new during setup, then reuses those presets for future version switches.
version: 0.1.1
author: CITIZENZ
license: MIT
metadata:
  hermes:
    tags: [node, nvm, nvm-windows, presets, frontend, windows]
---

# my-node-preset

Use this skill when the user wants to switch Node.js versions semantically:
- `old` for legacy projects
- `stable` for everyday projects
- `new` for modern projects

The actual version numbers are user-defined, not hardcoded.

## Use this skill for

- “switch to old node”
- “switch to stable node”
- “switch to new node”
- “what node version am I on?”
- “show installed node versions”
- “use this project’s node version”
- “read .nvmrc and switch”
- “switch to node 16.15.0”
- “show my node presets”
- “set stable to 20.11.1”
- “reset node presets”

## v1 support boundary

This lightweight v1 is primarily written for:
- Windows
- `nvm-windows`
- already-installed Node versions
- agent-driven usage

Do not present this as a full cross-platform Node manager.

## Preset persistence rule

Preset mappings are user-level local configuration for this machine and this agent environment.
They are not project-level settings.
They should persist across future sessions for the same user.

Project-specific version requirements should live in `.nvmrc`, not in the preset mapping.

## Initial setup rule

This skill should be initialized once after installation or first enablement.
Do not wait until the first preset switch request to define the presets if a setup step is available.

During initial setup, ask the user which Node version should map to:
- `old`
- `stable`
- `new`

Then save that mapping as user-level persistent local configuration for this machine and agent environment.

After setup is complete, future preset requests should use the saved mapping directly.

If the user skips setup and later asks for `old`, `stable`, or `new` with no saved mapping, then fall back to the same setup questions before switching.

If the user wants help choosing, suggest example values such as:
- `old` -> `14.18.2`
- `stable` -> `18.18.0`
- `new` -> `22.17.0`

These are examples only.

## Prerequisite checks

Before attempting a switch, verify:
- a supported Node version manager is available
- on Windows v1, prefer `nvm-windows`
- the target version string is known

If `nvm` is not installed or the environment is clearly not `nvm-windows`, fail explicitly and say this lightweight v1 mainly targets Windows with `nvm-windows`.

## Switching behavior

### Current version

When the user asks for current status, report:
- `node -v`
- `npm -v`
- actual `node` executable path if available

### Installed versions

When the user asks for installed versions, use the active version manager to list them.
On Windows with `nvm-windows`, prefer:
- `nvm list`

### Preset switch

When the user asks for `old`, `stable`, or `new`, resolve the saved preset and switch to that version.
On Windows with `nvm-windows`, the usual command is:
- `nvm use <version>`

### Explicit version switch

If the user asks for a specific version, use it directly.
Example:
- `nvm use 16.15.0`

If the requested version is not installed, v1 should fail clearly and explain that the version is missing.
Do not silently install it in lightweight v1.
At most, recommend installation as a separate next step.

### Project version

If the user asks to use the project version, prefer `.nvmrc`.
Search upward from the current directory or provided project path until `.nvmrc` is found.
If found, read the version and switch to it.
If not found, say so clearly and recommend adding `.nvmrc` at the project root.

For lightweight v1, `.nvmrc` support should focus on plain version forms first, such as:
- `16.15.0`
- `v18.18.0`
- `18`

Do not imply guaranteed support for alias forms like `lts/*` across all managers.

## Precedence rules

When multiple signals exist, use this precedence:

1. explicit version request wins
   - example: “switch to node 16.15.0”
2. explicit project-version request means `.nvmrc` wins
   - example: “use this project’s node version”
3. preset request means preset wins, even inside a repo with `.nvmrc`
   - example: “switch to stable”

## Preset management

If the user asks to inspect presets, show the current `old`, `stable`, and `new` mappings.
If the user asks to change one, update only that mapping.
If the user asks to reset presets, clear the saved mapping and require initial setup again on the next preset request.

## Required verification

After every attempted switch, verify the real runtime. Do not trust the switch command alone.

Always check:
- `node -v`
- `npm -v`
- actual `node` executable path

If the result does not match the requested version, report the mismatch explicitly.

## Windows guidance

This skill is especially useful on Windows with `nvm-windows`.
A common problem is that the symlink path lives under `C:\Program Files\nodejs`, which may cause permission or path issues.

Other common failure causes include:
- conflicting Node installations already on PATH
- stale PATH order
- a broken symlink target
- another version manager interfering with resolution

If switching fails:
1. Do not pretend it succeeded.
2. Show the real error.
3. Explain the most likely Windows causes.
4. If possible, show the current Node version and current executable path.

## Project guidance

Recommend that users keep a `.nvmrc` in each project root.
This makes project requirements explicit and makes agent behavior more predictable.

## Output style

Keep responses short, operational, and explicit.

Example success output:

Switched to stable
Node: v18.18.0
npm: 9.8.1
Path: C:\Program Files\nodejs\node.exe

Example failure output:

Switch failed
Target: stable -> 18.18.0
Reason: Access is denied
Current Node: v14.18.2
Path: C:\Program Files\nodejs\node.exe

## Summary

Let the user define `old`, `stable`, and `new` during setup, treat them as user-level presets, keep `.nvmrc` as the project-level source of truth, and always verify the real runtime after switching.
