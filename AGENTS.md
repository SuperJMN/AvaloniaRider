# AI Agent Guidelines

This repository maintains a "Divergent Fork" strategy to keep custom branding ("Avalonia Previewer") while tracking the upstream repository.

## Divergent Fork Strategy

Our `main` branch contains specific changes to `plugin.xml` (branding) and `gradle.properties` (versioning) that MUST be preserved.

### 1. Synchronization (Syncing with Upstream)
When asked to update from the original repository:
1.  Execute: `git pull upstream main`
2.  You will encounter merge conflicts in:
    *   `src/rider/main/resources/META-INF/plugin.xml`
    *   `gradle.properties`
3.  **Conflict Resolution**: ALWAYS accept **OUR** changes (Keep Current / Ours) for name and version.
    *   Keep `<name>Avalonia Previewer</name>` and `<id>avaloniapreviewer</id>` in `plugin.xml`.
    *   Keep our version in `gradle.properties`.
    *   Keep `val intellijPluginId = "avaloniapreviewer"` in `build.gradle.kts`.
    *   Accept upstream changes for everything else.

### 2. Contribution (Creating PRs for Upstream)
When asked to fix a bug or add a feature to contribute back to the original repo:
1.  **NEVER** create the PR branch from our `main` branch.
2.  Create a fresh branch from upstream:
    ```bash
    git fetch upstream
    git checkout -b <feature-branch-name> upstream/main
    ```
3.  Apply your fixes/features on this clean branch.
4.  Verify that `plugin.xml` and `gradle.properties` DO NOT contain our custom branding changes.
5.  Push and create the PR from this branch.

### 3. Releasing / Generating a New Version
To generate a new version of the **Avalonia Previewer** plugin:
1.  **Update Version**: Change `pluginVersionBase` in `gradle.properties`.
2.  **Update Changelog**: Add a new entry to `CHANGELOG.md` with the new version and its changes.
3.  **Local Build**: To generate the plugin ZIP locally, run:
    ```bash
    ./gradlew buildPlugin -PbuildConfiguration=Release -PbuildRelease=true
    ```
    The generated file will be located at `build/distributions/avaloniapreviewer-<version>.zip`.
4.  **Official Release**: Push a git tag following the format `v<version>` (e.g., `v2.1.0`). The GitHub Action `release.yml` will automatically build and publish the plugin to the JetBrains Marketplace if the credentials are set.
