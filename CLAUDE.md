# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Commands

### Build & Development

- `yarn gulp dev` - Start development server on port 3001 with auto-rebuild on changes
- `yarn gulp build` - Build application for development
- `yarn gulp release` - Build application for production
- `yarn gulp watch` - Watch files and rebuild incrementally
- `yarn hot` - Start webpack-dev-server with hot reloading
- `yarn start` - Start terriajs-server (requires built application)

### Linting & Code Quality

- `yarn prettier` - Format code using Prettier
- `yarn prettier-check` - Check code formatting
- `yarn gulp lint` - Run ESLint on index.js and lib/ directory

### Utilities

- `yarn gulp clean` - Remove build artifacts
- `yarn gulp sync-terriajs-dependencies` - Sync package dependencies with TerriaJS

## Architecture Overview

This is a TerriaJS-based geospatial web application using a monorepo structure with yarn workspaces.

### Core Structure

- **entry.js** - Application entry point with loading screen
- **index.js** - Main application bootstrap and TerriaJS initialization
- **lib/** - Custom application components and overrides
- **wwwroot/** - Static assets, configuration files, and build output
- **buildprocess/** - Webpack configuration and build utilities

### TerriaJS Integration

- Uses TerriaJS as primary dependency from custom fork: `github:pabrojast/terriajs#fe1a9b5121090ee1d6347c6a9388762b241b2d70`
- Extends TerriaJS with custom components in `lib/Views/`
- Overrides TerriaJS styling via `lib/Styles/variables-overrides.scss`
- Custom webpack configuration extends TerriaJS buildprocess

### Key Configuration Files

- **config.json** - Runtime application configuration
- **serverconfig.json** - terriajs-server proxy configuration
- **wwwroot/init/\*.json** - Catalog initialization files
- **buildprocess/webpack.config.js** - Main webpack configuration

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

- Node.js >= 20.0.0 required
- Uses TypeScript with React JSX support
- ESLint configuration includes React and TypeScript rules
- Prettier for code formatting (version 2.7.1)
- Husky for git hooks with pretty-quick for staged file formatting
