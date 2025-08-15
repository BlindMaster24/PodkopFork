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