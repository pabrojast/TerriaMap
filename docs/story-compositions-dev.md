# Story compositions in development

The TerriaJS revision pinned in `package.json` and `yarn.lock` adds optional version-1 `composition` metadata to native scenes and `storyOptions.displayMode` to shares. Existing stories keep their previous viewer until a composition is added. Editing, capture and recapture retain this metadata.

The editor offers narrative, map, dashboard, combined and media templates, text position/proportions, a preview and per-scene duration. Link selected narrative text to a saved scene or CKAN dashboard state. Scroll and Slides remain manually navigable; optional playback starts with Play, waits for map/dashboard completion, pauses on interaction/errors/hidden tabs, and stops at the end.

CKAN must serve `/story-dashboards/picker`, `/dashboard/<view-id>/embed`, `/story-images/upload` and `/story-images/library` on the same origin. The dashboard picker uses the existing CKAN session and permissions. Deploy the matching Pages release first. No new environment variables are required.

Build with the repository Dockerfile (`yarn install`, `yarn gulp clean`, `yarn gulp release --baseHref=/terria/`). For DEV use `Deploy Terria (dev)` in `ckan-unesco-docker`, with the full TerriaMap commit SHA and `deploy_branch=miserver-2.10`. This change does not authorize or trigger production deployment.

Validation before publication: 15 focused Terria browser specs, TypeScript and targeted ESLint passed. A local TerriaMap bundle against these sources verified template save, composition retention on recapture, real dashboard filters (20,000 to 3,334 synthetic rows), playback/pause/end and a 390px reader. Deployment and live examples are recorded in the deployment repository.

The DEV showcase revision improves chapter navigation and touch controls, removes the empty mobile media panel, and pauses native audio/video on chapter or reading-mode changes. Direct videos use metadata preload and inline playback.
