# rimago/rpi-kodi

Dockerized [Kodi](https://kodi.tv/) with audio and video for the raspberry pi4.

![Kodi screenshot](https://kodi.tv/sites/default/files/page/field_image/about--devices.jpg "Kodi screenshot")

## Features

* fully-functional [Kodi](https://kodi.tv/) installation in a [Docker](https://www.docker.com/) container
* **audio** ([ALSA or PulseAudio](https://kodi.wiki/view/Linux_audio)) and **video**
* simple, Raspbian image that adheres to the [official Kodi installation instructions](https://kodi.wiki/view/HOW-TO:Install_Kodi_for_Linux#Installing_Kodi_on_Ubuntu-based_distributions)
* clean shutdown of Kodi when its container is terminated

## Host Prerequisites

The host system will need the following:

1. **Linux** and [**Docker**](https://www.docker.com)

   This image should work on any Linux distribution with a functional Docker installation.
   
2. **A connected display and speaker(s)**
       
## Usage
* Create file docker-compose.yml

```yml
version: "3.7"
services:
  rpi-kodi:
    image: rimago/rpi-kodi
    container_name: "kodi"
    user: kodi
    network_mode: host
    restart: always
    privileged: true
    devices:
      - /dev/fb0:/dev/fb0
      - /dev/vchiq:/dev/vchiq
    volumes:
      - /home/pi/kodi/home:/home/kodi
      - "/etc/timezone:/etc/timezone:ro"
      - "/etc/localtime:/etc/localtime:ro"
    tmpfs:
      - /tmp
    environment:
      - PULSE_SERVER=127.0.0.1
```
WARNING: it requires the --privileged flag which is risky. Please let me know if you have an idea how to remove it.

 * If you want to have the home directory outside of the container (e.g. /home/pi/kodi/home:/home/kodi) you need to add the following user to the host:

```
   groupadd -g 9002 kodi && useradd -u 9002 -r -g kodi kodi
```
   and make sure this user is the owner of the home directory.
   
## Contributing
This docker project is based on [erichough/kodi](https://github.com/ehough/docker-kodi).

Constructive criticism and contributions are welcome! Please 
[submit an issue](https://github.com/rimago/rpi-kodi/issues/new) or 
[pull request](https://github.com/rimago/rpi-kodi/compare).
