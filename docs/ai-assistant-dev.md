# AI Assistant DEV delivery

The `feat/ai-assistant-20261030` branch pins TerriaJS with map proposals, explicit Apply/Undo, source imports and spatial catalog search. `dataAssistant` remains the configuration key. The matching terriassistant backend advertises capability version 1; the frontend keeps data-query compatibility with older backends.

Install with the repository's Yarn 1 release. `package.json` and `yarn.lock` pin the same immutable library SHA. No dependency versions other than that pin changed. The DEV deployment uses the existing same-origin CKAN session/API configuration and the dedicated `deploy/ai-assistant-dev-20261030` deployment branch. No merge or production promotion is part of this candidate.

Release and browser evidence: `ckan-unesco-docker/deploy/envoy-dev/validation-ai-assistant-2026-10-30.md`.

After candidate validation, TerriaJS PR #18 was merged into `develop` at `95e20e21d9915c2e6dfc98159c9792fc4dcc8d8f`; package.json and yarn.lock now pin that merge revision. Its runtime code is identical to the validated candidate. TerriaMap is integrated through PR #11 into `develop`; the canonical delivery workflow runs from `ckan-unesco-docker` branch `miserver-2.10` with `terriamap_ref=develop`. Backend commits remain immutable pins and are now ancestors of their canonical integration branches. This promotion targets DEV only.

## Separate dataset search (2026-10-30)

Library `311a6b27475b0500458d83883ad97171d4b462b9` moves catalogue search to Add data → Search datasets. It opens a side panel with optional text, polygon drawing and filters; resource additions preserve saved CKAN views. Search state survives tool changes and chat resets. Use this area in AI Assistant explicitly copies its polygon into the analysis context. The assistant keeps chat, collapsed context and Apply/Undo. My data handles file/URL imports on desktop and mobile; Share displays excluded layers. Existing configuration and same-origin CKAN endpoints are retained.

Validation and DEV release evidence: `ckan-unesco-docker/deploy/envoy-dev/validation-dataset-search-2026-10-30.md`.
