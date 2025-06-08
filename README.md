# gen-fstab-plus.sh

A Bash script to automate Btrfs subvolume management and filesystem setup on Ubuntu systems.

---

## Overview

This script automates the setup of a Btrfs-based filesystem layout on Ubuntu. It performs the following tasks:

- Mounts the root Btrfs partition.
- Creates a predefined set of Btrfs subvolumes, including user-specific directories like `~/bin` and `~/dev`.
- Ensures all necessary mount points exist.
- Sets appropriate ownership for user directories.
- Restores filesystem info from a backup `/etc/fstab.bak`.
- Generates a clean, reproducible `/etc/fstab` file.
- Optionally configures SMB/CIFS mounts, ensuring `cifs-utils` is installed.

It is designed to be run **after a fresh installation** to prepare the system with a consistent, maintainable filesystem layout.

---

## Requirements

- Ubuntu or Debian-based system with Btrfs root partition.
- Bash shell.
- `cifs-utils` package if you want to mount SMB/CIFS shares:
  ```bash
  sudo apt-get install cifs-utils
  ```


## Usage

Run the script with sudo:
```bash
sudo ./gen-fstab-plus.sh
```

The script will:

- Prompt for your username (default is root).
- Create necessary Btrfs subvolumes if they do not exist.
- Ensure mount points exist and have proper ownership.
- Mount subvolumes as configured.
- Generate a new /etc/fstab.new file and show a summary of its contents.
- Ask for confirmation before replacing /etc/fstab with the new file.
- Reload systemd daemon if /etc/fstab is replaced.

## Important Notes

- The script logs all major actions and decisions.
- It is idempotent and safe to run multiple times.
- Before mounting SMB/CIFS shares, it checks for cifs-utils and installs it if missing.
- It manages bind mounts such as /data/bin -> ~/bin and /data/dev -> ~/dev.

## Example Output

```bash
[2025-06-08 14:38:57] Script started.
Enter your username (default: root): afreisinger
[2025-06-08 14:39:01] Subvolume already exists: @cache
[2025-06-08 14:39:01] Mount point already exists: /var/cache
[2025-06-08 14:39:01] cifs-utils already installed.
[2025-06-08 14:39:01] New fstab saved to /etc/fstab.new

Summary of new /etc/fstab.new (non-comment lines):
UUID=4165948c-7049-46c8-ae0a-4e3938748c79 / btrfs subvol=@,ssd,noatime 0 0
UUID=4165948c-7049-46c8-ae0a-4e3938748c79 /var/cache btrfs subvol=@cache,compress=no 0 0
//192.168.1.20/bups /mnt/bups cifs credentials=/etc/cifs_credentials,iocharset=utf8 0 0

Do you want to replace /etc/fstab with this new version? (y/N)
```



## Source & License

Licensed under MIT License 

## Contribution

Feel free to fork, modify, and submit pull requests or issues!
