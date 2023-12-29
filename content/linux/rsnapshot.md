Title: Making backups using `rsnapshot`
Date: 2023-12-29
Tags: linux, backups
Summary: Now that I have two drives in my server, time to follow good practice and make backups


-------

## Backup Methods

As far as I can tell, there are two main paradigms of local backups. You either make an archive, and keep continuous archives, or you make plain copies of the files, but then as backups build you use links to point to the unchanged files from the last backup. Since I'm backing up a lot of family photos, I chose to use copies and links through `rsnapshot`, as that feels like it will use less space in the end.

## Setting up `rsnapshot`

After installing it through my package manager, it was relatively simple to setup. The default location for the config file is at `/etc/rsnapshot.conf`, and for my use case most of the defaults were fine. The only thing I had to change was to set `snapshot_root` to a folder called `backups` on my smaller hard drive, set the `retain` values to keep 6 daily backups, 7 weekly backups, and 4 monthly backups, and finally point the `backup` values to the folders I wanted to backup, being `/home` and `/mnt/black`

## Automating `rsnapshot`

To make this backup routines actually run on a regular schedule, I added them to the cron tab using `sudo crontab -e`. The specific rules I entered were:
```cron
# Run daily backups at 00:05 daily
5 0 * * *       /usr/bin/rsnapshot daily
# Run weekly backups at 01:05 on Sundays
5 1 * * SUN     /usr/bin/rsnapshot weekly
# Run monthly backups at 02:05 on the 1st
5 2 1 * *       /usr/bin/rsnapshot monthly
```

Notice that I scheduled each job to run at a different hour, to give each one plenty of time to not run into another one.

## Restoring a backup from `rsnapshot`

Finally, in the case that something goes wrong, we use `rsync` to restore backups made by `rsnapshot`, since that is what it's using in the background. The specific example I tested was having a folder at `/mnt/black/dummy` backed up to `/mnt/Vault/backups/daily.0/dummy/mnt/black/dummy`, which I restored using the command:
```bash
sudo rsync -av --numeric-ids --delete /mnt/Vault/backups/daily.0/dummy/mnt/black/dummy /mnt/black
```