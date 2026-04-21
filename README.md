# my-node-preset

A lightweight skill for AI agents that switches Node.js versions by personal meaning instead of memorized numbers.

You define three presets once:

- `old` — your legacy-project Node
- `stable` — your default everyday Node
- `new` — your modern-project Node

After that, the agent can understand requests like:
- “switch to stable”
- “switch to old”
- “use this project’s .nvmrc”

## What this solves

Many developers bounce between old and new frontend projects and keep mentally translating:
- this repo wants Node 14
- that one wants 18
- the new app wants 22

The underlying tools already exist (`nvm`, `nvm-windows`, `fnm`, `volta`), but the user interaction is still clunky. This skill adds a personal semantic layer on top of them.

Instead of remembering numbers every time, the user sets up their own meaning once, and the agent reuses it consistently.

## Quick example

Initial setup:
- `old` = `14.18.2`
- `stable` = `18.18.0`
- `new` = `22.17.0`

Later usage:

User:
switch to stable

Agent:
Switched to stable
Node: v18.18.0
npm: 9.8.1
Path: C:\Program Files\nodejs\node.exe

Project usage:

User:
use this project’s node version

Agent:
Read .nvmrc
Switched to Node v16.15.0
Node: v16.15.0
npm: 8.5.5
Path: C:\Program Files\nodejs\node.exe

## Who this is for

- developers juggling legacy and modern Node projects
- Windows users on `nvm-windows`
- AI-agent-driven workflows
- people who want one-time setup and lower day-to-day friction

## What this is not

- not a replacement for `nvm`
- not a full cross-platform Node manager yet
- not an automatic installer for missing Node versions in v1

## Design philosophy

This repository intentionally starts small:

- one public `SKILL.md`
- clear switching rules
- one-time preset initialization
- no required helper script in v1

The goal of v1 is to validate the interaction model first.

## v1 contract

This first public version is intentionally narrow.
It primarily targets:

- Windows
- `nvm-windows`
- already-installed Node versions
- agent-driven usage

It does not promise cross-platform parity yet.

## Initial setup behavior

The intended experience is a one-time setup step after installation or first enablement.
During setup, the user is asked which Node version should map to:

- `old`
- `stable`
- `new`

That mapping is then saved as user-level persistent local configuration for the current machine and agent environment.

After that, future preset switches should just work.

If setup did not run and the user later asks to use `old`, `stable`, or `new`, the skill should fall back to the same setup questions before switching.

Preset mappings are user-level convenience settings.
Project-specific requirements should still live in `.nvmrc`.

## Recommended initial examples

These are examples only, not hardcoded defaults:

- `old` -> `14.18.2`
- `stable` -> `18.18.0`
- `new` -> `22.17.0`

Different users may prefer:

- `old` -> `16.x`
- `stable` -> `20.x`
- `new` -> `22.x`

## What the skill supports

Core actions:

- show current Node environment
- list installed Node versions
- switch to `old`
- switch to `stable`
- switch to `new`
- switch to an explicit version like `16.15.0`
- switch using `.nvmrc`
- show current preset mappings
- update one preset mapping
- reset preset mappings

Project-aware guidance:

- recommend using `.nvmrc` in each project
- optionally infer a suggestion from `package.json` `engines`

## Explicit prerequisites

Before attempting a switch, the skill should verify:

- a supported version manager exists
- on Windows v1, `nvm-windows` is the preferred target
- the requested target version or preset can be resolved

If the environment does not appear to support this flow, the skill should fail explicitly instead of pretending to work.

## Missing-version policy

In lightweight v1, if the requested version is not installed:

- fail clearly
- explain that the version is missing
- optionally recommend installation as a separate next step
- do not silently install it

v1 only switches to versions that are already installed.

## .nvmrc support scope

In lightweight v1, `.nvmrc` support should focus on plain version forms first, such as:

- `16.15.0`
- `v18.18.0`
- `18`

Do not imply guaranteed support for alias values like `lts/*` across all managers in v1.

## Precedence rules

The skill should follow this order:

1. explicit version request wins
2. explicit “use this project’s version” means `.nvmrc` wins
3. preset request means preset wins, even inside a repo with `.nvmrc`

## Windows-specific risk notes

The most obvious issue is permissions when `nvm-windows` points through `C:\Program Files\nodejs`.
But that is not the only failure mode.

Other common causes include:

- conflicting Node installations already on PATH
- stale PATH ordering
- broken symlink targets
- another version manager interfering with resolution

The skill should report these as likely causes instead of reducing every failure to admin rights only.

## What makes this different from raw nvm

This is not trying to replace `nvm`.
It is a skill layer that gives agents and users a stable personal vocabulary.

Raw tool:
- `nvm use 18.18.0`

Skill-level request:
- “switch to stable”
- “use the Node version from this project’s .nvmrc”

## Roadmap

### v1

- lightweight public skill
- one-time preset initialization
- semantic switching rules
- verification rules after switching
- troubleshooting guidance for common Windows issues

### v2

- optional helper scripts
- standardized config file format
- stronger diagnostics
- support for more Node version managers
