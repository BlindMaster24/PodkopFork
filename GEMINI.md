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

To maintain a clear, consistent, and readable git history, this project follows a set of conventions for commit messages.

#### The 7 Rules of a Great Commit Message

These are the foundational rules for writing good commit messages, recommended by the Git community:

1.  **Separate subject from body with a blank line.**
2.  **Limit the subject line to 50 characters.**
3.  **Capitalize the subject line.**
4.  **Do not end the subject line with a period.**
5.  **Use the imperative mood in the subject line** (e.g., `Add feature` not `Added feature`).
6.  **Wrap the body at 72 characters.**
7.  **Use the body to explain *what* and *why* vs. *how*.**

#### Structure: Conventional Commits + Gitmoji

To structure the commits, we use the [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) specification, with the addition of [Gitmoji](https://gitmoji.dev/) for better readability.

The format is:
`<emoji> <type>(<scope>): <subject>`

*   **`<emoji>`:** An emoji that represents the change.
    *   `✨` for new features.
    *   `🐛` for bug fixes.
    *   `📝` for documentation changes.
    *   `♻️` for refactoring code.
    *   `🔖` for releases or version bumps.
    *   `🔧` for configuration changes.
    *   See the full list at [gitmoji.dev](https://gitmoji.dev).

*   **`<type>`:** The type of the change.
    *   `feat`: A new feature.
    *   `fix`: A bug fix.
    *   `docs`: Documentation only changes.
    *   `refactor`: A code change that neither fixes a bug nor adds a feature.
    *   `chore`: Changes to the build process or auxiliary tools.

*   **`<scope>` (optional):** The part of the codebase that is affected (e.g., `podkop`, `luci-app`, `readme`).

*   **`<subject>`:** A short, imperative-tense description of the change.

#### Example of a Full Commit Message

```
✨ feat(podkop): Add firewall helper for isolated zones

The new firewall helper automatically creates rules to accept Podkop's
marked traffic (0x105). This prevents packets from being dropped by
default REJECT policies and ensures seamless traffic flow.

The helper is smart enough to detect the correct firewall zone for
each configured interface.
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
