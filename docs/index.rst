==========================================
Setting up your Plex server and seedbox:
==========================================
*Alex B.*
*This is a guide for how you can connect a local Plex server to a remote seedbox, so that all downloading/seeding of files can take place on the seedbox but movies and tv shows can be stored and accessed locally on the Plex server.*

My Environment Setup:
-----------------------------

On my home network:
    Optiplex 9020 w/32 GB DDR3 RAM, Intel i7 CPU 8th gen, 1 TB SSD - primary, 6 TB 7200 RPM HDD - secondary

    Running on the Optiplex using docker containers from linuxserver repository https://docs.linuxserver.io/:
        - Plex
        - Radarr
        - Prowlarr
        - Deluge (included because it was needed to allow Radarr to pull .torrent files even if the actual files aren't being downloaded here)

Seedbox:
    From RapidSeedbox I used the $30 "Stream" option - 10 Gbps Network speed, 7 GB RAM, 2.2 TB Storage, 6 CPU cores, and dedicated IP address

    Running on remote seedbox:
        - Deluge
        - SSH/SFTP server


High-level Overview:
------------------------------
I can browse movies using Radarr, and select them just like I would if files were getting downloaded locally. This grabs the .torrent file and puts it into a certain directory.

A python script running as a cronjob on the local machine checks this directory for any .torrent files - if it finds any, they get uploaded via SFTP to the seedbox into a certain directory.

The seedbox directory is being watched by the seedbox Deluge, so when the .torrent files get uploaded from home the seedbox Deluge will immediately start to download them.

While the local machine is connected to the seedbox, it also checks another directory for any completed downloads. If it finds any that it doesn't already have, it grabs them.

