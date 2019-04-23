<h1 align="center">
	Alan
</h1>
<p align="center">
	<b>A</b>rch linux <b>LAN</b> based installer
</p>

Alan is a  utility tool designed to help quick installations of Arch linux through URL parameters and custom configurations.

## Installation

### TODO

## Usage
### Serverside

Alan is a serverside tool, to see usage information view its help page, `alan -h`.

### API

#### `GET` `/api/status`

Use this endpoint to check the status of the application, if all is well it should return a 200 with `ALAN IS OK`

#### `POST` `/api/lsblk`

A POST request to this endpoint must contain form data `lsblk` which has a value of `lsblk -J`.
The application will parse the lsblk json and return suggested devices.

### Clientside

Interactive installer:
 - `sh -C $(wget HOST:PORT/ -O -)`

Configuration based setup:
 - `sh -C $(wget HOST:PORT/cfg/<name> -O -)`

Dynamically constructed configuration (see [buildforms](#buildforms)):
 - `sh -C $(wget HOST:PORT/build?<buildform> -O -)`

## Buildforms

A dynamic configuration requires the user pass queary parameters with the `/build` endpoint.

| parameter        | type   | required |                            description                                    |
|------------------|--------|----------|---------------------------------------------------------------------------|
| default_password | string | true     | The password all users will have after the installation process is done   |
| boot_size        | uint   | true     | The size of the boot partition (default: 512MB)                           |
| root_size        | uint   | true     | The size of the root partition (default: 100% of drive)                   |
| disk             | string | true     | The disk to target ( /dev/sdx ) for installation                          |
| pacstrap         | string | false    | What packages to run pacstrap with (default: base base-devel)             |
| users            | string | false    | A comma separated list of users (format: `<USERANME>:<PASSWORD>&<GROUP>`) |
| packages         | string | false    | Packages to install via pacman at the end of the installation             |
| systemctl_enable | string | false    | systemctl services to enable at the end of the installation               |


## Example configuration file

```toml
[[config]]
name = "awesome"
target = ["/dev/sda", "/dev/mmcblk0"]
pacstrap = "base base-devel networkmanager"
swap-size = "2G"
bootloader = "grub2"

[[config.root]]
size = "100%"
fs = "ext4"

[[config.boot]]
size = "512MB"
fs = "fat32"

[[config.user]]
username = "steve"
default-password = "1234"
systemctl-enable = "NetworkManager"
```
