[![](https://img.shields.io/codacy/grade/ac35171da5ca4fc29cfcdd2f7c1f7833)](https://hub.docker.com/r/cm2network/squad/) [![Docker Build Status](https://img.shields.io/docker/cloud/build/cm2network/squad.svg)](https://hub.docker.com/r/cm2network/squad/) [![Docker Stars](https://img.shields.io/docker/stars/cm2network/squad.svg)](https://hub.docker.com/r/cm2network/squad/) [![Docker Pulls](https://img.shields.io/docker/pulls/cm2network/squad.svg)](https://hub.docker.com/r/cm2network/squad/) [![](https://img.shields.io/docker/image-size/cm2network/squad)](https://microbadger.com/images/cm2network/squad) [![Discord](https://img.shields.io/discord/747067734029893653)](https://discord.gg/7ntmAwM)
# Supported tags and respective `Dockerfile` links
-	[`latest` (*bullseye/Dockerfile*)](https://github.com/CM2Walki/Squad/blob/master/bullseye/Dockerfile)

# What is Squad?
Squad is a tactical FPS that provides authentic combat experiences through teamwork, communication, and gameplay. It seeks to bridge the large gap between arcade shooter and military simulation. Large scale, combined arms combat, base building, and a great integrated VoIP system. <br/>
This Docker image contains the dedicated server of the game. <br/>

> [Squad](http://store.steampowered.com/app/393380/Squad/)

<img src="https://vignette.wikia.nocookie.net/squadgame/images/2/27/Squad_logo.png/revision/latest?cb=20150625185705" alt="logo" width="300"/></img>

# How to use this image

## Hosting a simple game server
Running on the *host* interface (recommended):<br/>
```console
$ docker run -d --net=host -v /home/steam/squad-dedicated/ --name=squad-dedicated cm2network/squad
```

Running using a bind mount for data persistence on container recreation:
```console
$ mkdir -p $(pwd)/squad-data
$ chmod 777 $(pwd)/squad-data # Makes sure the directory is writeable by the unprivileged container user
$ docker run -d --net=host -v $(pwd)/squad-data:/home/steam/squad-dedicated/ --name=squad-dedicated cm2network/squad
```

Running multiple instances (iterate PORT, QUERYPORT and RCONPORT):<br/>
```console
$ docker run -d --net=host -v /home/steam/squad-dedicated/ -e PORT=7788 -e QUERYPORT=27166 -e RCONPORT=21115 --name=squad-dedicated2 cm2network/squad
```

**It's also recommended using "--cpuset-cpus=" to limit the game server to a specific core & thread.**<br/>
**The container will automatically update the game on startup, so if there is a game update just restart the container.**

### docker-compose.yml example
```dockerfile
version: '3.9'

services:
  squad:
    image: cm2network/squad
    container_name: squad
    restart: unless-stopped
    network_mode: "host"
    volumes:
      - /storage/squad/:/home/steam/squad-dedicated/
    environment:
      - MULTIHOME="" #IF YOY HAVE MULTIPLE IP ADDRESSES, PUT THE ONE YOU WANT TO WORK HERE
      - PORT=7787
      - QUERYPORT=27165
      - BEACONPORT=15000
      - RCONPORT=21114
      - FIXEDMAXPLAYERS=100
      - SERVER_NAME="Squad Dedicated Server"
```

# Configuration
## Environment Variables
Feel free to overwrite these environment variables, using -e (--env):
```dockerfile
PORT=7787
QUERYPORT=27165
BEACONPORT=15000
RCONPORT=21114
FIXEDMAXPLAYERS=80
FIXEDMAXTICKRATE=50
RANDOM=NONE
MODS="()"
SERVER_NAME="Squad Dedicated Server"

MULTIHOME=x.x.x.x - use only if you have multiple IP addresses enter server IP instead of x.x.x.x
```

## Config
The config files can be edited using this command:

```console
$ docker exec -it squad-dedicated nano /home/steam/squad-dedicated/SquadGame/ServerConfig/Server.cfg
```

If you want to learn more about configuring a Squad server check this [documentation](https://squad.gamepedia.com/Server_Configuration).

## Mods

Add each id to the MODS environment variable, for example `MODS="(13371337 12341234 1111111)"`

> MODS must be a bash array `(mod1id mod2id mod3id)` where each mod id is separated by a space and inclosed in brackets

You can get the mod id from the workshop url or by installing it locally and lookup the numeric folder name at `<root_steam_folder>/steamapps/workshop/content/393380`.

# Contributors
[![Contributors Display](https://badges.pufler.dev/contributors/CM2Walki/Squad?size=50&padding=5&bots=false)](https://github.com/CM2Walki/Squad/graphs/contributors)
