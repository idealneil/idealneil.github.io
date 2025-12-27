---
date: '2025-12-13T12:46:10+13:00'
draft: false
title: 'Portainer Docker Guide'
cover:
    image: images/portainer-docker.png
    alt: 'Portainer'
    caption: ''
    relative: false
tags: ["docker", "container", "management"]
categories: ["docker"]
---
# Portainer docker compose install guide

## Link to the [Portainer](https://www.portainer.io/) website
Portainer is a great tool to help manage your containers. If you're new to Docker, Portainer provides you with an easy to use graphical interface, instead of having to use the command line. The traditional method to install Portainer is with "docker run". I prefer to have this as part of my docker compose yml file.

### Assumptions:
You already have Docker intalled on your Linux server.

- Docker install [guide](https://docs.docker.com/engine/install/).
- Follow the post install [guide](https://docs.docker.com/engine/install/linux-postinstall/) and add your user to the docker group.

### Installation:
In your home directory create an App Data folder for all of your Docker config files. "appdata", "docker" are common folder names you can use.
```
mkdir appdata
cd appdata
mkdir portainer
```
Add the contents below file to your existing docker-compose.yml file
```
portainer:
    image: portainer/portainer-ce:latest
    container_name: portainer
    environment:
      - PUID=1000 #"id $user" to find your uid & gid values
      - PGID=1000
      - TZ=<TimeZone> #Set this to your local time zone "https://en.wikipedia.org/wiki/List_of_tz_database_time_zones"
    volumes:
      - /home/user/appdata/portainer:/data #change 'user' for the name of your account
      - /var/run/docker.sock:/var/run/docker.sock
    ports:
      - 9000:9000 #http access
      - 9443:9443 #https access
    restart: unless-stopped
```
Update your docker compose file in detatched mode.
```
docker compose up -d
```

Check the logs to see if the container deployed successfully.
```
docker logs portainer
```
Open a browser to the ip address of your server with port the number you set e.g. 192.168.1.100:9000

Then create an initial administrator account. Portainer suggests using 'admin' as the username, you can change this to another name that you want. If you don't want anonymous stats collected, untick this before you create the new admin user.

One environment variable I always update straight away is the 'Public IP' address. This makes it easier to open your application from within Portainer.

Under Administration, navigate to "Environment-related -> Environment -> local"

If you're accessing Portainer on your internal network, enter your internal address e.g. 192.168.1.100

For external servers enter the IP address or domain name for that server (this could even be your Tailscale address).

Happy Portainer-ing :)