# Podkop TODO List

This file outlines the planned tasks, feature requests, and ideas for the future development of Podkop.

## Refactoring

- [ ] **Consolidate Constants**: Refactor the `/usr/bin/podkop` script by moving all hardcoded values (like URLs, paths, and default settings) into a dedicated constants section at the top of the file. This improves maintainability and readability.
- [ ] **Eliminate Repetitions**: Identify and refactor repeated code blocks within `/usr/bin/podkop` into reusable functions or variables to adhere to the DRY (Don't Repeat Yourself) principle.
- [ ] **General Script Structure**: Evaluate and potentially restructure the main `/usr/bin/podkop` script for better readability, maintainability, and modularity. This could involve splitting the script into smaller, more focused files.

## Features & Enhancements

### Lists & Rules

- [ ] **Speedtest List**: Add a dedicated list for Speedtest services (e.g., domains used by speedtest.net, fast.com) to ensure they are routed correctly and do not interfere with proxy performance.

### Core Functionality

- [ ] **Decouple Tunnel Management**: Once the project Wiki is updated with comprehensive tunnel configuration guides, remove the built-in installation logic for VPNs (like AmneziaWG). The goal is to make Podkop a routing tool, not a VPN installer. An external script in a separate repository for AWG installation would be a good alternative.
- [ ] **Advanced Subscription Feature (Full Discussion Summary from GitHub Issue #118)**:
      > **Note:** The following is a detailed translation and summary of the discussion from GitHub Issue #118, initiated by `itdoginfo`, the owner of the original repository. It is preserved here for historical context. The final implementation in `podkop-ng` may differ.

      **1. `itdoginfo`'s Original Proposal**

      **Subscription Types:**
      The system must support two types of subscriptions:
      - **Native `sing-box` Outbounds:** Provided by services like Marzban and Remna. These are fetched by spoofing a user agent.
        ```sh
        curl -A "SFA/1.11.9" URL | jq
        ```
        - *Pros:* "As-is" integration without conversion.
        - *Cons:* Dependent on provider tags; new/deprecated features can cause issues.
      - **Base64 Encoded Link List:** The most common format, returned by default if the user agent is unknown.
        ```sh
        curl -A "podkop" URL | base64 -d
        ```

      **Subscription Handling Logic:**

      *   **Approach A (Manual Selection):**
          - Display all servers from the subscription and let the user choose one or more. The selected servers are then put into a `urltest`.
          - *Cons:* Difficult to implement in LuCI. It's not resilient; if a provider changes a server tag, the user's selection breaks.

      *   **Approach B (Filter-Based - Preferred):**
          - A more pragmatic and resilient approach. The user provides a filter keyword (e.g., `RU`).
          - The script fetches all outbounds and adds any server whose tag contains "RU" into a `urltest` group.
          - This is resilient to tag changes (e.g., "RU Moscow" changing to "RU Novosibirsk").
          - It should also support multiple filter conditions, like `SE|RU`.

      **Auto-Update Mechanism:**
      - Subscriptions need to be updated to keep the server list fresh.
      - This should be handled by a separate function called from a `crontab` job.
      - **Update Intervals:** Every 10 minutes, 1 hour, 6 hours, 24 hours, or never.

      **Explicitly Out of Scope:**
      - Do not implement failover logic within the Podkop script itself. Rely entirely on `sing-box`'s native capabilities (`urltest`).

      **2. Community Feedback and Ideas**

      *   **From user `ampetelin`:**
          - **On broken tags:** Instead of relying on provider tags in rules, all subscription outbounds could be put into a `selector` or `urltest` with a static, known tag. The user's rules would then point to this static tag, preventing breakage.
          - **On update frequency:** Frequent updates (every 10-60 minutes) are excessive because they require a `sing-box` restart. "Once a day" and "manual" options are sufficient. If a server URL changes, the user will be without a proxy for a day, but that's a reasonable trade-off. A smart restart (only if the config has not changed) could be a solution.

      *   **From user `m-nikitin`:**
          - **On complexity:** Acknowledged that implementing a full-featured UI for many servers is complex. Suggested that a more common use case is a simple primary server with a manually chosen backup server, which could be implemented as a simple `urltest`.
          - **Support for filters:** Independently came up with the same filter-based idea and expressed strong support for it, noting it's a "super" feature for fine-grained control.
- [ ] **Route All Router Traffic**: Add an option to route all outbound traffic originating from the router itself through the main proxy, in addition to the `br-lan` interface. This will require significant changes to the `nftables` rule set.
- [ ] **"Tunnel All" Mode**: Implement a "Tunnel All Traffic" mode. When enabled, all traffic from the LAN would be forced through the main proxy/VPN. This mode would likely disable FakeIP functionality and any extra routing sections, simplifying the routing logic for users who want maximum privacy.
- [ ] **"Source Format" Support**: Add support for "Source format" subscription lists. This requires parsing the format (likely JSON) and, if subnets are present, adding them to the custom subnet `nftset`.
- [ ] **Batch Processing for Custom Lists**: Refactor the function that processes custom domain/subnet lists. Instead of processing entries one by one, they should be batched to improve performance, especially when dealing with large lists.
- [ ] **Service Monitoring**: Implement a background monitoring process. After a successful start, the script should monitor the `sing-box` service. If `sing-box` crashes or exits with an error, the script should automatically restore the original `dnsmasq` configuration and attempt to restart the service. This could potentially be implemented using an `init.d` trigger.
- [ ] **DoH Blocking**: Add a simple option (e.g., a checkbox in LuCI) to block common DNS-over-HTTPS (DoH) servers, preventing clients on the network from bypassing the router's DNS-based routing rules.
- [ ] **IPv6 Support**: Plan and implement full IPv6 support. This is a major task that should be undertaken after the project Wiki is comprehensive and the existing IPv4 functionality is stable.

## Testing

- [ ] **Unit Tests**: Write unit tests for the shell script functions using a framework like BATS (Bash Automated Testing System) to ensure individual components work as expected.
- [ ] **Integration Tests**: Set up a backend integration testing environment using a minimal OpenWrt rootfs and BATS. This will allow for testing the end-to-end behavior of the `podkop` script in a realistic environment.
