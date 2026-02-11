# DM Logical Stack Visualizer

A small Python CLI that parses a TR-181 style `Device.*=value` dump (e.g., `DM.txt`) and prints the logical interface stack from top to bottom, plus a summary table.

## Requirements
- Python 3

## Usage
```bash
python3 show_logical_stack.py [DM.txt]
```
- If no file is provided, `DM.txt` in the current directory is used.

## Input format
The input file should contain lines like:
```
Device.Logical.Interface.1.Name="wan"
Device.Logical.Interface.1.LowerLayers="Device.IP.Interface.1"
```
Only `Device.*=value` lines are parsed; object header lines like `Device.Bridging.` are ignored.

## Output
- A per-logical-interface tree that follows `LowerLayers` references.
- A summary table showing role (WAN/LAN), status, and key lower-layer interfaces.
