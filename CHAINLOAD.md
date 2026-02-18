# Chainloading With `efibootmgr`

This project follows the UEFI `BootOrder` variable:
1. Firmware starts `DisablePROCHOT.efi` first.
2. `DisablePROCHOT.efi` chainloads the next boot entry in `BootOrder`.

That means you can configure chainloading directly in firmware boot entries without Clover/rEFInd.

## Prerequisites

- Linux with `efibootmgr` installed
- Booted in UEFI mode
- A valid UEFI boot entry for `DisablePROCHOT.efi`
- A valid UEFI boot entry for your normal OS loader

## 1) Find Your Boot Entries

List current entries:

```bash
sudo efibootmgr -v
```

Example output (shortened):

```text
BootCurrent: 0000
BootOrder: 0000,0001
Boot0000* Linux Boot Manager
Boot0002* DisablePROCHOT
```

In this example:
- `0002` = `DisablePROCHOT`
- `0000` = normal OS loader

## 2) Set `BootOrder`

Put `DisablePROCHOT` first and your normal loader second:

```bash
sudo efibootmgr --bootorder 0002,0000
```

You can include more entries after that as needed:

```bash
sudo efibootmgr --bootorder 0002,0000,0001,0003
```

## 3) Verify

Check that firmware accepted the change:

```bash
sudo efibootmgr
```

Confirm `BootOrder` starts with your `DisablePROCHOT` entry.

## Rollback

If needed, restore your previous order, for example:

```bash
sudo efibootmgr --bootorder 0000,0001
```

## Notes

- Entry IDs (`0002`, `0000`, etc.) are system-specific.
