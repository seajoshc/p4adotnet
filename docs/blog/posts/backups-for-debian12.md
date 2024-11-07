---
date:
  created: 2024-11-05
categories:
  - computers
tags:
  - sysadmin
---

# Setting Up Automated Backups with Restic on Debian Linux

[This article was created out of a convo with Claude 3.5 Sonnet.](https://chat.p4a.net/share/I1qk40xJNgzt7bRMQX62g)

Backups are crucial, but they need to be automated and reliable to be truly effective. In this guide, we'll set up a robust backup system using [restic](https://restic.net/) on [Debian Linux 12](https://www.debian.org/). We'll configure it to back up Docker volumes (/var/lib/docker/volumes), home directories (/home), and system configuration files (/etc), with snapshots every 12 hours and a sensible retention policy. We'll use systemd to schedule our backups, easily audit backups, and manually run a backup job when desired.

## Prerequisites

- Debian 12 (or similar Linux distribution)
- restic installed (`apt install restic`)
- root access or sudo privileges
- A backup destination (local disk, SFTP server, or S3 bucket)

## The Setup Process

We'll create four files to handle our backup system:

1. A backup script
2. An environment file for credentials
3. A systemd service
4. A systemd timer

Let's build this system piece by piece.

### 1. Creating the Backup Script

Create a new file at `/usr/local/bin/backup-script.sh`:

```bash
#!/bin/bash

# Exit on error
set -e

# Load environment variables
source /etc/restic-env

# Paths to backup
BACKUP_PATHS="/var/lib/docker/volumes \
             /home \
             /etc \
             /usr/local/etc"

# Initialize the repository if it doesn't exist
restic snapshots || restic init

# Perform the backup
restic backup $BACKUP_PATHS

# Keep the last 6 snapshots (72 hours worth, given 12-hour frequency)
# Keep 1 snapshot per week for all other snapshots
restic forget --keep-last 6 --keep-weekly 1 --prune
```

Make the script executable: `sudo chmod +x /usr/local/bin/backup-script.sh`

### 2. Setting Up the Environment File

Create `/etc/restic-env` to store your credentials and configuration:

```
# Repository location - adjust this to your needs
# Examples:
# export RESTIC_REPOSITORY="sftp:user@host:/srv/restic-repo"
# export RESTIC_REPOSITORY="s3:s3.amazonaws.com/bucket_name"
# export RESTIC_REPOSITORY="/path/to/local/repo"
export RESTIC_REPOSITORY="<YOUR_REPOSITORY_LOCATION>"

# Repository password
export RESTIC_PASSWORD="<YOUR_STRONG_PASSWORD>"

# If using S3:
# export AWS_ACCESS_KEY_ID="<YOUR_ACCESS_KEY>"
# export AWS_SECRET_ACCESS_KEY="<YOUR_SECRET_KEY>"
```

Secure the environment file: `sudo chmod 600 /etc/restic-env`

### 3. Creating the Systemd Service

Create /etc/systemd/system/restic-backup.service:

```
[Unit]
Description=Restic backup service
After=network.target

[Service]
Type=oneshot
ExecStart=/usr/local/bin/backup-script.sh
User=root
Group=root

# Set reasonable security options
ProtectSystem=full
ProtectHome=read-only
PrivateTmp=true
NoNewPrivileges=true
```

### 4. Setting Up the Timer

Create `/etc/systemd/system/restic-backup.timer`:

```
[Unit]
Description=Run restic backup every 12 hours

[Timer]
OnCalendar=_-_-\* 00,12:00:00
RandomizedDelaySec=1800
Persistent=true

[Install]
WantedBy=timers.target
```

### 5. Activating the Backup System

Enable and start the timer:

```
sudo systemctl daemon-reload
sudo systemctl enable restic-backup.timer
sudo systemctl start restic-backup.timer
```

## Backup Schedule and Retention

This setup:

- Creates backups every 12 hours (at midnight and noon)
- Adds a random delay of up to 30 minutes to avoid exact timing
- Keeps the last 72 hours of backups (6 snapshots)
- Maintains one weekly backup for older data
- Automatically prunes old snapshots

## Useful Commands

- Check the timer status: `sudo systemctl list-timers restic-backup.timer`
- Run a backup manually: `sudo systemctl start restic-backup.service`
- View backup logs: `sudo journalctl -u restic-backup.service`
- List all snapshots: `sudo bash -c 'source /etc/restic-env; restic snapshots' `
- Restore files: `sudo restic restore latest --target /path/to/restoration/directory`

## Security Considerations

The setup includes several security measures:

- The environment file is restricted to root access only
- The systemd service runs with restricted privileges
- The backup script uses safe bash options
- File systems are mounted read-only during backups where possible

## Testing Your Backups

Remember the cardinal rule of backups - they're only as good as your ability to restore from them. Regularly test your backup restoration process by:

- Creating a test directory
- Restoring a backup to it
- Verifying the restored files

## Conclusion

You now have a robust, automated backup system that:

- Runs automatically every 12 hours
- Maintains a sensible backup history
- Includes important system directories
- Uses secure settings
- Can be easily monitored and managed

Remember to periodically verify your backups and adjust the retention policy based on your specific needs. Happy backing up!
