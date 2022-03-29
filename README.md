# docker-zerotier-server

## Overview

**Features：**

- Fully functional, privately deployed Zerotier planet server with [Official Controller](https://github.com/zerotier/ZeroTierOne) and [Controller UI (Zero-UI)](https://github.com/dec0dOS/zero-ui).
- Automatically detect the public ipv4 address of the server and generate the planet file to download.
- One-click deployment using `docker-compose` with small size docker image.
- Support switch China source to build.



## Getting Started

### Prerequisites

- A VPS with a static public ip (Open the following ports: `4000/tcp` and `9993/udp`)
- `docker` and `docker-compose` installed

### Installation

1. Clone this repo to your VPS

   ```shell
   $ git clone https://github.com/hamlinzheng/docker-zerotier-server.git
   ```

2. Modify the installation version of `ZeroTier`(default: 1.8.6) and `Zero-UI`(default: 1.2.1) in `docker-compose.yml`

3. Build the images

   ```shell
   $ docker-compose build
   ```

4. Run the containers in daemon

   ```shell
   $ docker-compose up -d
   ```

5. Navigate to `http://[PUBLIC_IP]:4000/app/` to config your private ZeroTier service.

### Note:

- This project does not provide HTTP reverse proxy, which needs to be deployed on the host machine.
- Since the public network ip of the server needs to be detected, it needs to be compiled on the deployed server
- A static key pair is used by default. You can use `zerotier-idtool generate identity.secret identity.public` command to generate a new key pair on the machine where `ZeroTier` is installed according to your own needs, and replace the `config` folder.



## Peers Usage

Download private `planet` file from `http://[PUBLIC_IP]:4000/app/static/planet` and use it replace the peer's planet file. Then you can connect to your own `ZeroTier` planet server.

### Linux

```shell
$ wget http://[PUBLIC_IP]:4000/app/static/planet -O /var/lib/zerotier-one/planet
$ sudo systemctl restart zerotier-one.service
```

### Windows

Replace the planet file in the `C:\ProgramData\ZeroTier\One` path and restart the `ZeroTier` service.

### Android

See [kaaass](https://github.com/kaaass)/**[ZerotierFix](https://github.com/kaaass/ZerotierFix)**

### OpenWrt

Replace the planet file in the `/etc/config/zero` path and restart the `ZeroTier` service with `$ /etc/init.d/zerotier restart`



## TODO

- [ ] Optimize the docker image size of zero-ui
- [ ] Support IPv6
- [ ] Support domain



## Acknowledgement

- [zerotier](https://github.com/zerotier)/**[ZeroTierOne](https://github.com/zerotier/ZeroTierOne)**
- [dec0dOS](https://github.com/dec0dOS)/**[zero-ui](https://github.com/dec0dOS/zero-ui)**
- [sbilly](https://github.com/sbilly)/**[docker-zerotier-controller](https://github.com/sbilly/docker-zerotier-controller)**
- [opopop880](https://gitee.com/opopop880)/**[zerotier_planet](https://gitee.com/opopop880/zerotier_planet)**







