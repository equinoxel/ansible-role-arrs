# laurivan.arrs

This role installs Sonarr, Lidarr, Jackett and Deluge in Docker for a perfect combo.

## Requirements

None

## Role Variables

All variables are listed below (see also `defaults/main.yml`).

### Common variables

You need to specify:

- The `timezone`
- The location where torrents will be downloaded
- The location where different configuration files are stored
- The place where `docker-compose.yml` and the environment files are stored

```yml
timezone: 'Europe/Brussels'
torrent_downloads_volume: '/mnt/download'
arrs_configuration_volume: '/mnt/config'
arrs_setup_path: '~/arrs'
```

If you need to install the containers with a specific user/group ID, then define:

```yml
arrs_uid:
arrs_gid:
```
The role allows oyu to specify which components will be installed:

```yml
deluge_enabled: true 
sonarr_enabled: true 
lidarr_enabled: true 
jackett_enabled: true
```

### Deluge torrent

You can specify the image version and the log level:

```yml
deluge_image_version: 'latest'
deluge_loglevel: 'warning'
```

Deluge works on ports 6881 and 8112. You can change these ports:

```yml
deluge_host_port: 6881
deluge_admin_port: 8112
```

You can also overwrite the location where deluge's configuration is stored (e.g. if you already have deluge installed and you want to use the Ansible role):

```yml
deluge_config_volume: '{{ arrs_configuration_volume }}/deluge'
```

### Sonarr

You can specify the image version and the port exposed:

```yml
sonarr_image_version: 'latest'
sonarr_host_port: 8989
```
You can also overwrite the location where sonarr's configuration is stored (e.g. if you already have it installed and you want to use the Ansible role):

```yml
sonarr_config_volume: '{{ arrs_configuration_volume }}/sonarr'
```

Sonarr needs a place to copy the downloaded series:

```yml
sonarr_series_volume: '/mnt/videos/Series'
```

**Notes**:

- Depending on your settings, it will also rename your current series
- You need write access to that directory, so Sonarr can actually copy the files

# Lidarr

You can specify the image version and the port exposed:

```yml
lidarr_image_version: 'latest'
lidarr_host_port: 8686
```
You can also overwrite the location where lidarr's configuration is stored (e.g. if you already have it installed and you want to use the Ansible role):

```yml
lidarr_config_volume: '{{ arrs_configuration_volume }}/lidarr'
```
Lidarr needs a place to copy the downloaded music:

```yml
lidarr_music_upload_volume: '/mnt/music/Reference'
```

You will need to add a reference to your music collection (so you don't download what you already have). The layout below allows for multiple collections:

```yml
lidarr_music_volumes: 
  - {path: '/mnt/music/Sonos', alias: 'sonos' }
  - {path: '/mnt/music/Audiophile', alias: 'audiophile' }
  - {path: '/mnt/music/Raw', alias: 'raw' }
```

The `path` is the actual directory where the collection is located and the `alias` is the internal mapping name in Docker.

# Jakett
You can specify the image version, the port exposed and to autoupdate:

```yml
jackett_image_version: 'latest'
jackett_auto_update: true
jackett_host_port: 9117
```
You can also overwrite the location where jackett's configuration is stored (e.g. if you already have it installed and you want to use the Ansible role):

```yml
jackett_config_volume: '{{ arrs_configuration_volume }}/jackett'
```

## Dependencies

You need a machine with docker and docker-compose installed.

## Example Playbook

```yml
- hosts: servers
  roles:
      - 'laurivan.arrs'
```

## License

MIT

##  Author Information

This role was created in 2022 by [Laur Ivan](https://www.laurivan.com).
