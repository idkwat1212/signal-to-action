# First target: 433 MHz device

## Why this one

- rtl_433 already decodes 400+ device types out of the box. If rtl_433 knows it, the loop is trivial — use it as a smoke test.
- The interesting case is a device rtl_433 does *not* know. Then REA has to do the real work: recover the protocol from raw samples.
- Cheap, legal to own, easy to replace if something breaks.

## Hardware

- RTL-SDR Blog V3 or V4 dongle (~$30)
- 433 MHz antenna (or the stock whip)
- Raspberry Pi 4/5 or any Linux box with USB

## Software

```bash
# rtl_433
sudo apt install rtl-433
rtl_433 -f 433920000 -s 250000 -g 42 -R 0  # raw mode, all decoders off

# Or with JSON output for REA to chew on:
rtl_433 -f 433920000 -s 250000 -F json:433.json
```

## Success criteria

1. rtl_433 produces a JSON or text decode of the unknown signal.
2. REA reads that capture and extracts: modulation, bit timing, frame structure, address/command fields.
3. Agent writes a Python script using rpitx or an ESP32 that reproduces the frame.
4. The target device responds identically to the original remote/sensor.
5. All evidence committed to this repo.
