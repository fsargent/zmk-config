# Felix’s Ferris Sweep

Four-layer, 34-key Sweep / Cradio config for two nice!nano v2 controllers.
Firmware and the reusable build workflow are pinned to upstream ZMK
`4dd95d7840eb3d5a3f59bb29fbf1ab054715ef85` (Zephyr 4.1).
The historical boardsource5x12 configs are retained but are not built.

## Build with GitHub Actions

Push config changes or use **Actions → Build Sweep firmware → Run workflow**.
Download the successful run’s `firmware` artifact; it contains
`felix-sweep-left.uf2` and `felix-sweep-right.uf2`.
Only the left/central build enables ZMK Studio (USB and BLE).

## Initial upgrade and bootloader access

This assembled keyboard has no usable physical reset buttons. While the
old working firmware is still on both halves, stage both bootloaders:

1. Connect USB to the right half. Hold both left thumbs and tap slash.
2. Connect USB to the left half. Hold both left thumbs and tap Z.
3. Flash the matching UF2 to each half’s NICENANO bootloader drive.

If both drives are mounted, identify them by controller serial:
left `842AA5C13B65DD18`, right `F5F4B7E58E58F365`. Mount suffixes are not
reliable identifiers. Stage both before flashing either during an upgrade
that may temporarily interrupt split communication. Do not use a settings
reset image unless deliberately recovering pairing state.

UF2 flashing requires a computer that permits mounted bootloader drives.
Studio remapping uses USB serial or BLE instead; it does not mount a drive.
Keep the working pre-upgrade firmware as recovery images.

## Studio on a work computer

After the initial flash, connect the **left** half by USB and open
https://zmk.studio/ in Chrome or Edge. The browser must allow USB serial.
Hold both left thumbs, tap **U** for USB output, then **S** to unlock Studio.
Select Felix Sweep in Studio and save your edits to persist them.
The native Studio app also supports Bluetooth connections.

Studio changes individual key bindings; combos and behavior timing still
come from `config/cradio.keymap` and `config/combos.dtsi`. Studio-saved
keymaps override future source keymap changes until **Restore Stock
Settings** is used. The generated practice page describes the source
keymap, so it does not reflect edits made only in Studio.

Soft off is intentionally disabled because the assembled keyboard has no
verified physical wake path. Idle is 30 seconds and deep sleep is 90 minutes;
normal deep sleep still wakes on a keypress.

## Layout reference

See [Sweep layout notes](config/SWEEP.md) and [the practice page](sweep-keymap.html).
Regenerate and preview locally:

```sh
python3 scripts/generate-sweep-keymap.py
python3 -m http.server 8000 --bind 127.0.0.1
```

Open http://localhost:8000/sweep-keymap.html. Use `--pending` with the
generator while source changes have not yet been flashed.
