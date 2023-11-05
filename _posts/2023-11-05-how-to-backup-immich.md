---
layout: post
title: How to backup Immich
description: "How to backup Immich and restore it in case of a disaster."
---

# Whys

About a year ago I started my journey into homelabbing, and ever since I've been looking at what else I can play with or even make part of my homelab and daily routine. Having photos self-hosted sounds interesting enough and it also hits the mark in my idea of _owning_ my files. Before that I was using Google Photos and Yandex Disk to store my photos, but Google compresses them and Yandex is a Russian company, which is not a good thing in my opinion. So, I decided to move all my photos to my own server and use [Immich](https://immich.app/) to manage them.

# Whats

I have around 100k photos and videos and I certainly don't want to lose them. Considering I'm using pretty old hardware as my home server, it's nice to have an off-site backup for all my files. In this post I'll provide a brief overview of how I'm handling my Immich backups, so you could use my experience to own your photos in a safe way. Let's go!

# Hows

## Backups

Let's assume, you're already have Immich [installed](https://immich.app/docs/install/docker-compose) and running. Now, how would you setup your backups? And more important, how would you restore them in case of a disaster?

First things first: the backup. Immich docs offer an idea of [how to backup the database](https://immich.app/docs/administration/backup-and-restore), but I'll go further and backup the whole Immich directory, so I could restore it on a new server in case of a disaster. I'm running my Immich instance on Docker, so I'll use that, and you can adapt the instructions to your use case.

First, we would need to make a database backup. Immich uses PostgreSQL to store all the data, so we'll need to make a dump of the database. Following command will create a dump of the database for us:

```
docker exec -t immich_postgres pg_dumpall -c -U postgres | gzip > "/home/user/apps/immich/immich_dump.sql.gz"
```

Don't forget to create a directory for the dump file, otherwise you'll get an error.

After creating the DB backup, we would need to backup the Immich directory itself. We don't actually need to copy the whole images directory, we can just upload it to the cloud along with the database backup file. I use combination of [Rclone](https://rclone.org/) and [Backblaze B2](https://www.backblaze.com/b2/cloud-storage.html) to store my backups, but you can use any other cloud storage provider you like and use this post just as an example. Rclone is a great tool, which allows you to sync files between your local machine and the cloud. It supports a lot of cloud storage providers, so you can choose the one you like. I use Backblaze B2, because it's cheap and it has S3-compatible API, which is supported by Rclone.

Let's install Rclone on our server. I'm using Ubuntu, so I'll use apt to install it:

```
sudo -v ; curl https://rclone.org/install.sh | sudo bash
```

We also need to configure Rclone to use Backblaze B2. You can find instructions on how to do that [here](https://rclone.org/b2/). After configuring Rclone, we can sync our Immich directory to the cloud. I'm using following command to do that:

```
/usr/bin/rclone sync /home/user/apps/immich/ b2:immich-backup-bucket --config /home/user/.config/rclone/rclone.conf -P
```

Note, that I'm using absolute paths here, so you'll need to change them to match your setup. Also, I'm using `-P` flag to show the progress of the sync process. You can omit it if you don't want to see the progress. Here I'm using `[rclone sync](https://rclone.org/commands/rclone_sync/)` command, which will sync the source directory to the destination directory. It will also delete files from the destination directory, if they were deleted from the source directory. If you don't want that, you can use `[rclone copy](https://rclone.org/commands/rclone_copy/)` command instead. Be aware, that B2 charges you for the number of API calls, so if you have a lot of files, you might want to use `copy` command instead of `sync` to avoid unnecessary API calls. In that case you would probably need to somehow handle deleted files yourself.

## Restoring backups

Now, that we have our backups, let's see how we can restore them. First, we would need to restore the database backup. We can do that with the following command:

```
docker-compose down -v  # CAUTION! Deletes all Immich data to start from scratch.
docker-compose pull     # Update to the latest version of Immich (if desired)
docker-compose create   # Create Docker containers for Immich apps without running them.
docker start immich_postgres    # Start Postgres server
sleep 10    # Wait for Postgres server to start up
gunzip < "/path/to/backup/dump.sql.gz" | docker exec -i immich_postgres psql -U postgres -d immich    # Restore Backup
docker-compose up -d    # Start remainder of Immich apps
```

After restoring the database, we would need to restore the Immich directory. We can do that by providing a path to the directory with the backup files in the Immich config file. Since I use Docker, all I need to do is change the `UPLOAD_LOCATION` variable in my `docker-compose.yml` file to point to the directory with the backup files. After that, I can start my Immich instance and it will use the backup files to restore the data.

It might be a good idea to run a job to regenerate all the thumbnails and other metadata after restoring the backup. It can be done from Immich's [administration panel](https://immich.app/docs/administration/jobs).

## Conclusion

That's it! Now you know how to back up and restore your Immich instance. I hope you'll find this post useful and it will help you to own your photos in a safe way.

# Automation

To make life a bit easier for myself, I automated creating an Immich instance and setting backups using Ansible. My playbook will:

1. Create a directory for Immich
2. Upload docker-compose.yml to a server
3. Upload .env file to a server
4. Set up UPLOAD_LOCATION to a desired directory on a server
5. Start Immich in Docker
6. Install rclone
7. Upload rclone config file to a server
8. Schedule a cron job to backup Immich DB daily to a local directory
9. Schedule a cron job to sync Immich directory to the cloud once a month (it's cheaper)

To use it, download my [homelab repo](https://github.com/Freika/homelab), create `inventory.txt` from `inventory.txt.example`, and replace server IP address and variables to yours (in [immich] and [immich:vars] sections), create `tmp/rclone.conf` file with B2 config in the repo root directory and run `make install_immich`.

You can experiment with the playbook the way you want and adjust it to your needs.

Hopefully, it will help you to own your photos in a safe way. If you have any comments, feel free to reach me using email (in the footer of this page).
