# GEMINI Project Overview: podkop-ng

## Project Overview

**Name:** Podkop

**Purpose:** A networking tool for OpenWrt routers designed to manage and route internet traffic. It provides features for bypassing censorship, accessing geo-restricted content, and enhancing privacy.

**Core Technologies:**

*   **Backend:** Shell script (`/usr/bin/podkop`)
*   **Frontend:** LuCI (JavaScript)
*   **Networking:** `sing-box`, `dnsmasq`, `nftables`

**Architecture:** The project consists of two main parts:

1.  A core set of scripts and configuration files that run on the OpenWrt router.
2.  A LuCI web interface for easy configuration and management.

## Building and Running

The project is an OpenWrt package and is intended to be installed on an OpenWrt router.

**Installation:**

The `README.md` provides the following installation command:

```sh
sh <(wget -O - https://raw.githubusercontent.com/itdoginfo/podkop/refs/heads/main/install.sh)
```

**Building from Source:**

The project can be built from the source using the OpenWrt SDK. The `Makefile` in the `podkop` and `luci-app-podkop` directories are used for this purpose.

**Running:**

The `podkop` service can be managed using the init script:

```sh
/etc/init.d/podkop start
/etc/init.d/podkop stop
/etc/init.d/podkop restart
/etc/init.d/podkop reload
```

## Development Conventions

*   The project follows the standard OpenWrt package structure.
*   The backend is written in POSIX shell script (`/bin/ash`).
*   The frontend is a LuCI application written in JavaScript.
*   Configuration is managed using UCI (Unified Configuration Interface).
*   Traffic is routed using `sing-box` and `nftables`.
*   DNS is managed by `dnsmasq`.
*   The main logic is in `/usr/bin/podkop`, which is a large shell script. It uses `uci` to read configuration, `jq` to manipulate JSON for `sing-box`, and `nft` to configure the firewall.
*   The LuCI application in `luci-app-podkop` provides a user-friendly interface to configure the `podkop` service. It uses standard LuCI and JavaScript practices.

## Interaction Guidelines

*   **Language:** Please respond in Russian. Be direct, comprehensive, and don't hesitate to provide detailed, honest feedback.

## Development Process and Conventions

This section documents the development process and conventions established for this project. All content in this file, including this section, should be written in **English**.

### Commit Messages

To maintain a clear and consistent git history, and to avoid potential encoding issues, please follow these guidelines for commit messages:

*   **Use English:** All commit messages must be written in English.
*   **Use Conventional Commits:** Follow the [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) specification. The format is:
    ```
    <type>(<scope>): <subject>
    ```
    *   **type:** `feat` (new feature), `fix` (bug fix), `docs` (documentation), `style` (formatting), `refactor` (code refactoring), `test` (adding tests), `chore` (build process, admin).
    *   **scope (optional):** The part of the codebase that is affected (e.g., `podkop`, `luci-app`, `readme`).
    *   **subject:** A short, imperative-tense description of the change.

*   **Examples:**
    ```
    feat(podkop): Add firewall helper for isolated zones
    docs: Update installation instructions in README.md
    chore: Bump version to 0.4.7
    ```

### Release Process

To create a new release, follow these steps:

1.  **Update Version:** Increment the `PKG_VERSION` in both `podkop/Makefile` and `luci-app-podkop/Makefile`.
2.  **Update Changelog:** Add a new release section to `CHANGELOG.md` with a summary of the changes. Follow the format from [Keep a Changelog](https://keepachangelog.com/en/1.1.1/).
3.  **Commit Changes:** Commit the version and changelog updates with a message like `chore: Bump version to x.y.z`.
4.  **Create Tag:** Create a new **lightweight** git tag (without an annotation message). The tag name must start with `v` (e.g., `v0.4.7`).
    ```sh
    git tag vx.y.z
    ```
5.  **Push Changes:** Push the commit and the new tag to the remote repository.
    ```sh
    git push origin main --tags
    ```
6.  **Verify:** Pushing a new tag will trigger the GitHub Actions workflow. Go to the "Actions" tab in the repository to monitor the build process. If the build is successful, a new release with the compiled `.ipk` packages will be created automatically.

### Development Environment

*   **Line Endings:** The project uses Unix-style line endings (LF). If you are on Windows, it is recommended to configure git to handle line endings correctly to avoid issues.
    ```sh
    git config --global core.autocrlf true
    ```
