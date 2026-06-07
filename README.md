# luks-killholders

A safe, interactive helper script for Ubuntu/Linux that kills processes holding LUKS/mapper devices open.

Especially useful when your LUKS USB pendrive refuses to eject because of `gvfsd-metadata` or other processes.

### Features

- Interactive menu — lists all `/dev/mapper` devices
- Clearly highlights your **root filesystem** in red (with warnings)
- Filters out dangerous low-level devices (`control`, `dm_crypt-*`)
- Asks for confirmation before acting
- Uses `fuser -k` to kill all holding processes + cleans `gvfsd-metadata`
- Early sudo prompt

### Installation

```bash
# Download
curl -L -o ~/bin/kill-holders https://raw.githubusercontent.com/YOURUSERNAME/luks-killholders/main/kill-holders

# Make executable
chmod +x ~/bin/kill-holders

# Add ~/bin to PATH (if not already)
echo 'export PATH="$HOME/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
