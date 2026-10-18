# The signal-to-action loop

## Stage 1: Capture

Tools: rtl_433, sigrok-cli, tshark, or a raw SDR recording.

Goal: a file on disk — pcap, sr (sigrok), or rtl_433 JSON — containing an unknown or partially understood signal.

Rules:
- Record at least 30 seconds of idle + 30 seconds of the device doing its thing (button press, state change).
- Keep the original raw file; never overwrite it.
- Note the device, distance, antenna, and any visible LEDs or behavior.

## Stage 2: Understand

Tool: REA via MCP or CLI.

REA can already handle:
- Recorded packets (tshark decode-as rules, field extraction)
- Recorded logic signals (sigrok decoder annotations)
- Native binaries (pseudocode, assembly, strings, calls)
- Firmware (binwalk/unblob extraction + native handoff)
- Android APKs (JADX decompile + manifest)
- Process behavior (PTY capture, run comparisons)

What REA does *not* do: decide what the signal means in the physical world, or write the actuator code. That is stage 3.

## Stage 3: Act

The agent writes controller code from REA's findings:
- Protocol frame format (preamble, sync, address, command, checksum)
- Timing (bit width, pulse widths)
- Any rolling codes or pairing state

Target runtimes: Python on a Pi, MicroPython on ESP32, or plain Arduino C.

## Stage 4: Verify

- Replay the captured frames with the new controller.
- Compare the device's response to the original.
- Log pass/fail with the raw files as evidence.
- Commit everything: capture, REA output, controller, test log.
