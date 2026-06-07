#!/bin/bash
# Save as: ~/bin/kill-holders
# chmod +x ~/bin/kill-holders

# Ask for sudo early
sudo -v

# Detect root filesystem
ROOT_DEVICE=$(mount | grep "on / " | awk '{print $1}' | sed 's|/dev/||')

echo "=== Active /dev/mapper devices ==="

# List devices but filter out control and dm_crypt-* entries
mapfile -t devices < <(ls -1 /dev/mapper/* 2>/dev/null | \
    grep -v -E '/control$|/dm_crypt-' | sort)

if [ ${#devices[@]} -eq 0 ]; then
    echo "No suitable devices found in /dev/mapper"
    exit 1
fi

# Show list with ROOT warning
for i in "${!devices[@]}"; do
    dev="${devices[$i]}"
    if [[ "$dev" == *"$ROOT_DEVICE"* ]] || mount | grep -q "on / " | grep -q "${dev#/dev/}"; then
        echo -e "[$((i+1))] \e[1;31m${dev}  ← ROOT FILESYSTEM (DANGER)\e[0m"
    else
        echo "[$((i+1))] ${dev}"
    fi
done

echo "[$(( ${#devices[@]}+1 ))] Cancel"

echo -e "\n\e[1;33m⚠️  Warning: Killing processes on the ROOT FILESYSTEM can freeze or crash your system!\e[0m"

read -p "Choose device to kill holders: " choice

if ! [[ "$choice" =~ ^[0-9]+$ ]] || [ "$choice" -lt 1 ] || [ "$choice" -gt "$(( ${#devices[@]}+1 ))" ]; then
    echo "Invalid selection."
    exit 1
fi

if [ "$choice" -eq "$(( ${#devices[@]}+1 ))" ]; then
    echo "Cancelled."
    exit 0
fi

DEVICE="${devices[$((choice-1))]}"

echo -e "\nSelected: $DEVICE"

# Strong warning if root
if mount | grep -q "on / " | grep -q "${DEVICE#/dev/}"; then
    echo -e "\e[1;31m==================================================================\e[0m"
    echo -e "\e[1;31m⚠️  DANGER: This is your ROOT FILESYSTEM (/)\e[0m"
    echo -e "\e[1;31mKilling processes here may crash your entire system!\e[0m"
    echo -e "\e[1;31m==================================================================\e[0m"
fi

read -p "Are you sure? (y/N): " confirm
if [[ ! "$confirm" =~ ^[Yy]$ ]]; then
    echo "Cancelled."
    exit 0
fi

echo "Killing processes holding it open..."

sudo fuser -vmk "$DEVICE"
pkill gvfsd-metadata 2>/dev/null || true

echo "Done."
