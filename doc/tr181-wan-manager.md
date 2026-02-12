# TR-181 X_PRPLWARE-COM_WANManager — WAN Modes

## References

- [Broadband Forum TR-181 Data Model](https://device-data-model.broadband-forum.org/)
- [prpl Foundation wan-manager plugin (GitLab)](https://gitlab.com/prpl-foundation/components/core/plugins/wan-manager)

## Overview

`Device.X_PRPLWARE-COM_WANManager` manages WAN mode profiles. Each `WAN.{i}`
describes a WAN configuration (physical type + interface set), and each
`WAN.{i}.Intf.{j}` describes an IP-facing interface attached to that mode.

Key top-level parameters:

| Parameter | Description |
|---|---|
| `OperationMode` | Manual/Automatic selection of WAN modes |
| `SensingPolicy` | AtBoot or Continuous sensing |
| `SensingTimeout` | Sensing timeout in seconds |
| `WANMode` | Active mode alias (matches `WAN.{i}.Alias`) |

## Object Hierarchy

```
Device.X_PRPLWARE-COM_WANManager
└── WAN.{i}
    ├── Alias / Status / Origin
    ├── PhysicalType / PhysicalReference
    ├── DNSMode / IPv6DNSMode
    ├── EnableSensing / SensingPriority
    └── Intf.{j}
        ├── Alias / Name / Type / VlanID
        ├── IPv4Mode / IPv6Mode
        ├── IPv4Reference / IPv6Reference
        ├── DHCPv4Reference / DHCPv6Reference
        └── DefaultRouteReference
```

## Active Mode Selection

The active WAN mode is determined by `WANMode`, which stores the **Alias** of a
`WAN.{i}` instance. The visualizer marks the active mode with a `★` indicator.

## Reference Aliases

Interface references (e.g. `Device.IP.Interface.2`) are annotated with their
resolved `Alias` or `Name` so you can quickly identify targets.

## Reading dm_visualizers/show_wan_manager.py Output

The script prints:

1. **Overview box**: OperationMode, SensingPolicy, SensingTimeout, and active WANMode
2. **Per-mode diagram**: Each WAN mode with status, physical type, and interfaces
3. **Summary table**: All modes with alias, status, sensing, and interface count

Layout adapts to terminal width:

- **Compact (< 90 cols)**: card layout with per-interface reference lines
- **Wide (≥ 90 cols)**: table layout with columns for modes and interfaces
