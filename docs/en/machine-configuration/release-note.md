---
weight: 11
queries:
  - machine configs release notes
  - machine-config version history
  - what changed in machine-config
---

# Release Notes

## Overview

This page tracks the published versions of the machine-config plugin.

## v4.0.18 (2026-08-20) \{#v4-0-18}

This release hardens access control, refreshes the resource schemas, and updates dependencies to address security vulnerabilities.

### Fixed Issues

- Removed the service-account Secret from the plugin RBAC template.
- Refined MachineConfigPool CRD descriptions and generated MachineConfig, MachineConfigPool, and MachineConfiguration schemas while retaining the public resources.
- Updated Go dependencies, checksums, and controller/daemon build metadata to address CVEs.

## v4.0.16 (2026-06-24) \{#v4-0-16}

This release extends machine configuration management to both Global and business clusters and includes security dependency updates.

### New Features

- Removed the chart restriction that prevented deployment in Global and business clusters, allowing the plugin to be installed in both cluster scopes.

### Security Updates

- Updated Go module dependencies, checksums, and the controller image base to address CVEs.

## v4.0.13 (2026-03-12) \{#v4-0-13}

The first published release introduces declarative node configuration for files, system services, and kernel settings, with controlled node disruption and configuration drift detection.

### New Features

- Added declarative node configuration through MachineConfig, MachineConfigPool, and MachineConfiguration resources.
- Added management of files, system services, kernel configurations, node draining, configuration rendering, and on-disk drift monitoring.
- Added the machine-config controller, node daemon, CRDs, Helm chart, RBAC, and build configuration.
- Added a dedicated daemon health check and removed obsolete metrics and probe network ports.
- Added a webhook, serving certificates, and registration to prevent unsafe MachineConfigPool deletion.
- Corrected MachineConfig selector handling and parsing for custom MachineConfigPools.
- Added chart workload security settings and refreshed controller/daemon RBAC, including CIS hardening changes.

### Security Updates

- Updated Go dependencies, checksums, image bases, and build configuration to address CVEs.
- Removed obsolete on-disk validation and configuration-drift helper code.
