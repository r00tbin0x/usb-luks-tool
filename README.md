
# usb-luks-tool

A safe, interactive helper script for Ubuntu/Linux that identifies and removes processes preventing LUKS-encrypted USB drives from being unmounted, locked, or ejected.

Especially useful when a LUKS USB drive refuses to safely eject because of `gvfsd-metadata`, file managers, terminal sessions, or other processes holding the device open.

### Features

* Interactive menu — lists available `/dev/mapper` devices
* Clearly highlights your **root filesystem** in red (with warnings)
* Filters out dangerous low-level devices (`control`, `dm_crypt-*`)
* Asks for confirmation before taking action
* Uses `fuser -k` to terminate processes holding the selected device open
* Cleans up common desktop-service locks such as `gvfsd-metadata`
* Early sudo prompt

