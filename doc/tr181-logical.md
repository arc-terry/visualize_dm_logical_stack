# TR-181 Device.Logical — Interface Stack

## References

- [Broadband Forum TR-181 Data Model](https://device-data-model.broadband-forum.org/)
- [prpl Foundation tr181-logical plugin (GitLab)](https://gitlab.com/prpl-foundation/components/core/plugins/tr181-logical)

## Overview

`Device.Logical` models logical interfaces and their stacking relationships using
the TR-181 InterfaceStack concept. Each `Logical.Interface.{i}` points to its
lower layers via `LowerLayers`, which usually references IP and Ethernet objects.

Key top-level parameter:

| Parameter | Description |
|---|---|
| `InterfaceNumberOfEntries` | Count of logical interfaces |

## Object Hierarchy

```
Device.Logical
└── Interface.{i}
    ├── Alias / Name / Status
    ├── LowerLayers            ← References to IP.Interface, Ethernet.Link, ...
    ├── X_PRPLWARE-COM_WAN.Status
    └── X_PRPLWARE-COM_LAN.Status
```

## Role Resolution

The visualizer derives a role for each logical interface:

- **WAN** when `X_PRPLWARE-COM_WAN.Status == "Enabled"`
- **LAN** when `X_PRPLWARE-COM_LAN.Status == "Enabled"`
- **?** when neither status is enabled

## Interface Stack Walk

`LowerLayers` is a comma-separated list of object references. The script walks
the first chain of lower layers recursively and prints a tree:

```
Logical.Interface.N
└── IP.Interface.X
    └── Ethernet.Link.Y
        └── Ethernet.Interface.Z
```

The displayed label prefers `Name`, then falls back to `Alias`, and includes
`Status` when available.

## Reading dm_visualizers/show_logical_stack.py Output

The script prints:

1. **Per-interface header**: Logical interface ID, name, and role
2. **Stack tree**: A top-to-bottom walk of `LowerLayers`
3. **Summary table**: One line per logical interface with:

| Column | Meaning |
|---|---|
| `#` | Logical interface ID |
| `Alias` | Logical alias |
| `Role` | WAN/LAN/? |
| `Status` | Logical interface status |
| `IP Interface` | First IP layer in the stack |
| `Eth Link` | First Ethernet link in the stack |
| `Bottom Layer` | Deepest resolved layer (with name, if available) |
