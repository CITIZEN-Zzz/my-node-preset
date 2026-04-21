# Changelog

## 0.1.1

Refined the lightweight public contract.

Changes:
- shifted from first-use preset setup to one-time setup after installation or first enablement
- clarified that presets are user-level local configuration, not project-level settings
- updated author attribution to CITIZENZ
- added explicit prerequisite checks
- added missing-version failure policy for v1
- narrowed `.nvmrc` expectations for lightweight v1
- added precedence rules between explicit version, project version, and presets
- expanded Windows risk notes beyond simple admin-rights issues
- added preset management intents: show, update, reset

## 0.1.0

Initial lightweight public draft.

Includes:
- semantic Node presets: old, stable, new
- first-use preset configuration flow
- current-version inspection guidance
- installed-version listing guidance
- explicit-version switching guidance
- project-aware `.nvmrc` switching guidance
- Windows and `nvm-windows` troubleshooting notes

Scope of this version:
- lightweight skill layer only
- no mandatory helper script yet
- intended to validate the public interaction model first
