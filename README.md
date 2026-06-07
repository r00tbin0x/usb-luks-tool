
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

### Installation

```bash
# Download
curl -L -o ~/bin/usb-luks-tool https://raw.githubusercontent.com/YOURUSERNAME/usb-luks-tool/main/usb-luks-tool

# Make executable
chmod +x ~/bin/usb-luks-tool

# Add ~/bin to PATH (if not already)
echo 'export PATH="$HOME/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

### Usage

```bash
usb-luks-tool
```

Select the LUKS device you want to release, review the warnings, and confirm the action. The script will identify and terminate processes preventing the encrypted USB drive from being safely unmounted, locked, or ejected.
