# Agent Instructions for FireLabs Helm Common Library

This document provides guidance for AI agents (like Claude Code) when making changes to this Helm library chart.

## Overview

This is a Helm **library chart** - it is not meant to be installed directly. Instead, it provides reusable templates and helpers for other Helm charts to depend on.

## Key Principles

1. **Backward Compatibility**: Changes should maintain backward compatibility whenever possible. Breaking changes require a major version bump.
2. **Template-First**: All functionality should be implemented as reusable templates in the `templates/` directory.
3. **Documentation**: All changes must be documented in both the README.md and values.yaml file.
4. **Testing**: Changes should not break existing charts that depend on this library.

## File Structure

```
.
├── Chart.yaml                 # Chart metadata (version, description, maintainers)
├── values.yaml               # Default values with extensive documentation
├── README.md                 # Main documentation
├── AGENTS.md                 # This file - agent instructions
├── CHANGELOG_LEGACY.md       # Historical changelog from k8s-at-home fork
└── templates/
    ├── _*.tpl                # Core templates (deployment, service, ingress, etc.)
    ├── lib/                  # Library helpers (chart, controller utilities)
    ├── classes/              # Class-based templates for resources
    └── addons/               # Add-on sidecars (code-server, vpn, netshoot, promtail)
```

## Making Changes

### 1. Updating Chart Version

When making changes, update the version in `Chart.yaml` following [Semantic Versioning](https://semver.org/):

- **MAJOR** (x.0.0): Breaking changes that require users to modify their values
- **MINOR** (5.x.0): New features, non-breaking enhancements
- **PATCH** (5.0.x): Bug fixes, documentation updates

### 2. Adding New Features

When adding new features:

1. **Add to values.yaml**: Document the new value with inline comments
2. **Create/update templates**: Implement the feature in the appropriate template file
3. **Update README.md**: Add the new values to the values table and changelog
4. **Test**: Verify the feature works with a test chart

Example workflow:
```yaml
# values.yaml
addons:
  newAddon:
    # -- Enable the new add-on
    enabled: false
    # -- Configure new add-on image
    image:
      repository: ghcr.io/example/addon
      tag: latest
```

### 3. Template Organization

- **Core templates** (`_deployment.tpl`, `_service.tpl`, etc.): Main resources
- **Library helpers** (`lib/chart/*.tpl`): Reusable helper functions
- **Classes** (`classes/*.tpl`): Resource class implementations
- **Addons** (`addons/*/`): Sidecar containers and related resources

### 4. Naming Conventions

- Template files: `_name.tpl` (underscore prefix prevents Helm from rendering them as resources)
- Template definitions: `common.names.function` or `common.addon.name.function`
- Values: Use camelCase for keys

### 5. Adding a New Addon

To add a new sidecar addon (e.g., "claudecode"):

1. Create addon directory: `templates/addons/claudecode/`
2. Create required files:
   - `_claudecode.tpl`: Main addon template
   - `_container.tpl`: Container definition
   - `_volume.tpl`: Volume configuration (if needed)
   - `_secret.tpl`: Secret handling (if needed)
3. Add values to `values.yaml` under `addons.claudecode`
4. Update README.md with new addon documentation

Example addon structure:
```
templates/addons/claudecode/
├── _claudecode.tpl      # Main template that includes others
├── _container.tpl       # Container spec
├── _volume.tpl          # Volume definitions
└── _secret.tpl          # Secret for API keys
```

### 6. Documentation Requirements

Every change must update:

1. **values.yaml**: Inline comments for all new/changed values
2. **README.md**:
   - Update version badge
   - Add to changelog
   - Update values table (can be auto-generated)
3. **Chart.yaml**: Bump version

## Common Tasks

### Adding Support for a New Kubernetes Resource Type

1. Create `templates/_resourcetype.tpl`
2. Define template: `{{- define "common.classes.resourcetype" -}}`
3. Add conditional rendering in `templates/_all.tpl`
4. Document in values.yaml

### Fixing a Bug

1. Identify the affected template file
2. Make the fix
3. Bump patch version in Chart.yaml
4. Add entry to README.md changelog
5. Test with existing charts

### Adding Configuration Options

1. Add to `values.yaml` with documentation comments
2. Update template to use the new value
3. Bump minor version in Chart.yaml
4. Update README.md with new value and changelog entry

## Testing

Before committing changes:

1. **Syntax validation**: Run `helm lint .` to check for syntax errors
2. **Template rendering**: Test with `helm template test-release .`
3. **Integration test**: Create a test chart that depends on this library and verify it works

## Commit Guidelines

- Use conventional commit messages: `feat:`, `fix:`, `docs:`, `chore:`
- Reference issues when applicable
- Keep commits focused and atomic
- **Never add co-author tags** - commits should be authored by the maintainer only

## Publishing

The chart is automatically published via GitHub Actions when changes are pushed to the `main` branch:

1. GitHub Actions workflow builds the chart package
2. Chart is published to GitHub Pages at `https://fireball1725.github.io/firelabs-helm-common/`
3. Users can add the repository and install the chart

## Questions or Issues?

- Check existing issues: https://github.com/FireBall1725/firelabs-helm-common/issues
- Review k8s-at-home documentation for patterns: https://github.com/k8s-at-home/library-charts
- Test changes thoroughly before committing

## AI Agent Specific Notes

When you (an AI agent) are asked to modify this chart:

1. **Always read Chart.yaml first** to understand the current version
2. **Read the relevant template files** before making changes
3. **Update all three locations**: Chart.yaml version, values.yaml documentation, README.md changelog
4. **Ask for clarification** if the requested change could break backward compatibility
5. **Suggest testing** for significant changes
6. **Follow the principle of least surprise** - keep changes consistent with existing patterns

Remember: This is a library chart used by other charts. Changes here can affect many downstream projects, so be careful and thorough!
