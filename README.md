# signal-to-action

Agent loop that captures a signal, understands it, and drives hardware to respond.

This is the gap none of REA, DeepSeek Harness, or Scientific Agent Skills close: REA reads binaries, the harness orchestrates, the skills guide analysis — but nothing chains capture → understanding → physical action.

## Stack

- **REA** (morluto/rea) — reverse engineering agent, MCP + CLI. Analyzes native binaries, JavaScript/Electron apps, Android APKs, firmware, recorded packets, recorded logic signals, process behavior.
- **rtl_433** — decodes 433 MHz RF signals from sensors, remotes, weather stations.
- **sigrok** — logic analyzer / protocol decoder for captured signals.
- **tshark** — packet capture analysis.
- **Target hardware** — SDR dongle (RTL-SDR), ESP32/Arduino, relay boards, smart plugs.

## The loop

1. **Capture** — SDR or logic probe records an unknown RF/logic/packet signal from a device.
2. **Understand** — REA (via MCP) inspects the capture: decodes protocol fields, traces the binary that generated it, recovers the command structure.
3. **Act** — Agent writes a controller (Python/MicroPython) that speaks the recovered protocol and drives the hardware.
4. **Verify** — Run the controller, compare behavior against the original device, log evidence.

## First target

A cheap 433 MHz smart plug or sensor. Capture with rtl_433, hand the capture to REA, rebuild a controller from what it finds.

## Status

- [ ] REA installed and registered with agent
- [ ] rtl_433 + SDR dongle working
- [ ] First capture recorded
- [ ] REA analysis of capture
- [ ] Controller written and tested
- [ ] Evidence log committed

## Why this matters

Every other AI-RE project stops at understanding. This one closes the loop back to the physical world.
