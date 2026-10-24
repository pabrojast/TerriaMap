# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Commands

### Build & Development

- `yarn gulp dev` - Start development server on port 3001 with auto-rebuild on changes
- `yarn gulp build` - Build application for development
- `yarn gulp release` - Build application for production
- `yarn gulp watch` - Watch files and rebuild incrementally
- `yarn start` - Start terriajs-server (requires built application)

### Linting & Code Quality

- `yarn prettier` - Format code using Prettier
- `yarn prettier-check` - Check code formatting
- `yarn gulp lint` - Run ESLint (flat config `eslint.config.mjs`, reused from terriajs) on index.js and lib/ directory

### Utilities

- `yarn gulp clean` - Remove build artifacts
- `yarn gulp sync-terriajs-dependencies` - Sync package dependencies with TerriaJS

## Architecture Overview

This is a TerriaJS-based geospatial web application built with yarn 1 (`.yarn/releases/yarn-1.22.22.js`).

### Core Structure

- **entry.js** - Application entry point; `lib/Views/render.jsx` mounts a React 18 root with a loader while `lib/Views/terriaStore.ts` imports and starts `index.js`
- **index.js** - Main application bootstrap and TerriaJS initialization
- **lib/** - Custom application components and overrides
- **wwwroot/** - Static assets, configuration files, and build output
- **buildprocess/** - Webpack configuration and build utilities

### TerriaJS Integration

- Uses TerriaJS as primary dependency from custom fork: the revision pinned in `package.json` (branch `develop` of `pabrojast/terriajs`, flat layout, synced with upstream TerriaJS 8.13.0+; `main` of both repos stays untouched until `develop` is deployed and verified). `terriajs-cesium` in `devDependencies` must match the version terriajs declares (26.3.0), otherwise `gulp build` aborts with two copies of Cesium.
- Upstream TerriaMap (`https://github.com/TerriaJS/TerriaMap`, remote `upstream`) is merged into the fork periodically; keep the fork's `index.ejs` title, `404.html`, `serverconfig.json` proxy list, Helm values, `simple.json` catalog and the git dependencies when resolving conflicts.
- Extends TerriaJS with custom components in `lib/Views/`
- Overrides TerriaJS styling via `lib/Styles/variables-overrides.scss`
- Custom webpack configuration extends TerriaJS buildprocess

### Key Configuration Files

- **config.json** - Runtime application configuration
- **serverconfig.json** - terriajs-server proxy configuration
- **wwwroot/init/\*.json** - Catalog initialization files
- **buildprocess/webpack.config.js** - Main webpack configuration (takes `{ devMode, baseHref }`; `html-webpack-plugin` renders `wwwroot/index.ejs` to `wwwroot/index.html`, so there is no `render-index` gulp task anymore)

### CKAN Integration

The image is served at `/terria` on the same origin as CKAN (dev `data.dev-wins.com`, production `ihp-wins.unesco.org`), and one image serves both. Every CKAN URL is therefore root-relative: the public catalog in `wwwroot/init/simple-modular.json` (`/api/terria/file/full`) and the `parameters.ckanSession` block in `wwwroot/config.json` (`/api/terria/user/...`). Do not make them absolute: cookie-authenticated calls must not go through the terriajs-server proxy, which drops `Cookie`. Under `yarn gulp dev` there is no CKAN on the origin, so the IHP-WINS group fails to load locally.

### Development Workflow

1. Run `yarn gulp dev` to start development server
2. Application builds automatically on file changes
3. Access at http://localhost:3001
4. Build artifacts output to `wwwroot/build/`

### Plugin System

- Plugin loading implemented via `lib/Core/loadPlugins.ts`
- Plugin configuration in `plugins.ts` (referenced but not included)
- Supports terriajs-plugin-api for extensibility

### Styling Architecture

- Uses SCSS with CSS modules
- TerriaJS variables can be overridden in `lib/Styles/variables-overrides.scss`
- Global styles in `lib/Views/global.scss`
- Webpack handles SCSS compilation and CSS extraction

### Build Output

- **TerriaMap.js** - Main application bundle
- **TerriaMap.css** - Compiled stylesheet
- Assets copied from TerriaJS to `wwwroot/build/TerriaJS/`

## Development Notes

- Node.js >= 22 required (Docker images use `node:22`)
- Uses TypeScript with React JSX support
- ESLint configuration includes React and TypeScript rules
- Prettier for code formatting (version 3.8.1)
- Husky for git hooks with pretty-quick for staged file formatting
