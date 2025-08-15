# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.1/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---
*This project is a fork of the original [itdoginfo/podkop](https://github.com/itdoginfo/podkop) project. The goal of this fork is to continue the development, improve the existing functionality, and gradually become an independent project.*
---

## [Unreleased]

### Added
-

### Changed
-

### Fixed
-


## [0.4.7] - 2025-08-15

### Added
- **Firewall Helper:**
  Introduced a smart firewall helper that automatically creates rules to accept Podkop's marked traffic. It detects the correct firewall zone for each configured interface and adds a specific `ACCEPT` rule for packets marked with `0x105`. This prevents them from being dropped by default `REJECT` policies and ensures seamless traffic flow. The helper also cleans up all created rules on stop.


