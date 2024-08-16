

This document is intended to provide a `docker-compose` of the Docker Containers that I have TRIED but NOT used on my Synology DS1513+/Mini-PC home server, as I did not find any use for me. If you are using a different device, that should be fine as well, just make sure you define the `$PERSIST` location in your `.env` file that corresponds to the docker folder on your host device.

If you are not familiar with how to use a compose file, you can change the compose to docker CLI command by using a [docker-compose Converter](https://bucherfa.github.io/dcc-web/)
 

Below are some ports that I have found to be reserved and cannot be used on my host, I always avoid using them

##### A CONSOLIDATED `unused-docker-compose.yml` FILE CAN BE FOUND [HERE](unused-docker-compose.yml) , AND TO START, JUST RENAME THE FILE TO _`docker-compose.yml`_ and TYPE IN `docker-compose up -d` AND ALL CONTAINERS WILL START. NOTE THAT SOME CONTAINERS REQUIRES CONFIGURATION BEFORE STARTING, AS WELL AS MAKING SURE ALL REQUIRED FOLDERS ARE CREATED ON THE HOST MACHINE WITH THE CORRESPONDING NAMES AS IN THE COMPOSE 


 #### _RESERVED PORTS NOT BE USED IN ANY CONTAINER_
 <details open="open">
  <summary>
  </summary>

 ```
 80        HTTP
 443       HTTPS
 1234      SSH
 1986      DSM HTTP
 1991      DSM HTTPS
 5050      Unknown Port Used
 5432      Unknown Port Used
 7575      Reserved for the Docker Dashboard (personal choice)
 8080      TCP/UDP
```
</details>


# DATABASES
 
| Container                                                       | Port       | Description                                        | Docker Image                         |
| :------------                                                   | :----      | :-------                                           | :---                                 |
| <a href="#2fauth-redis">2FAuth-Redis</a>                        | 6378       | Used for 2FAuth                                    | `redis:alpine`                       |
| <a href="#chatvault-postgres">Chatvault-Postgres</a>            | -          | Used for Chatvault                                 | `postgres:alpine`                    |
| <a href="#grafana">Grafana</a>                                  | 3003       | An open observability platform                     | `grafana/grafana:main`               |
| <a href="#joplindb">JoplinDB</a>                                | -          | Used for Joplin                                    | `postgres:13.1`                      |
| <a href="#our-shopping-list-db">Our Shopping List-DB</a>        | 27017      | Used for Our Shopping List                         | `pmongo:4.0`                         |
| <a href="#phpmyadmin">phpMyAdmin</a>  ----  Personally replaced with <a href="https://github.com/tamimology/docker-containers#adminer">Adminer</a>                              | 3300       | UI interface to view self-hosted DBs               | `phpmyadmin/phpmyadmin:latest`       |
| <a href="#phlare ">Phlare </a>                                  | 4100       | Continuous observability for workload's resources  | `grafana/phlare:latest`              |
| <a href="#photoview-postgress">Photoview-Postgress</a>          | -          | Used for Photoview                                 | `postgres:alpine`                    |
| <a href="#piped-postgres">Piped-Postgres</a>                    | -          | Used for Piped                                     | `postgres:alpine`                    |
| <a href="#piwigo-db ">Piwigo-DB</a>                             | -          | Used for Piwigo                                    | `jbergstroem/mariadb-alpine:10.6.13` |
| <a href="#prometheus">Prometheus</a>                            | 9090       | Systems and service monitoring system              | `prom/prometheus:latest`             |



# DOCKER RELATED

| Container                                                       | Port       | Description                                        | Docker Image                         |
| :------------                                                   | :----      | :-------                                                 | :---                           |
| <a href="#alertmanager">AlertManager</a>                        | 9093       | Handles alerts sent by client applications (Prometheus)  | `prom/alertmanager:latest`     |
| <a href="#auto-heal">Autoheal</a>                                  | -               | Monitor and restart unhealthy docker containers      | `modem7/docker-autoheal:latest`        |
| <a href="#broadlinkmanager">BroadLinkManager</a>                | 7020       | Helps to work with Broadlink Devices                     | `techblog/broadlinkmanager:latest` |
| <a href="#cadvisor">cAdvisor</a>                                | 8888       | Analyzes resource usage and performance of running containers | `gcr.io/cadvisor/cadvisor:latest` |
| <a href="#checkmk">CheckMK</a>                                  | 8722       | Powerful monitoring of networks, servers, containers     | `checkmk/check-mk-raw:latest`  |
| <a href="#container-mon">Container-Mon</a>  ----  Personally replaced with <a href="https://github.com/tamimology/docker-containers#monocker">Monocker</a>                      | -          | Get notified when a Docker containers are unhealthy      | `ghcr.io/rafhaanshah/container-mon:latest`                     |
| <a href="#decompose">DeCompose</a>                              | 4321       | Generates docker-compose file from existing containers   | `techblog/decompose:latest`    |
| <a href="#deunhealth">DeUnHealth</a>  ----  Personally replaced with <a href="https://github.com/tamimology/docker-containers#auto-heal">Autoheal</a>                           | -          | Restarts unhealthy containers                            | `qmcgaw/deunhealth:latest`     |
| <a href="#dockge">DockGE</a>                                    | 6001       | Easy-to-use self-hosted docker compose.yaml manager      | `louislam/dockge:1`            |
| <a href="#glances">Glances</a>                                  | 61208      | Cross-platform monitoring tool                           | `joweisberg/glances:latest`    |
| <a href="#node-exporter">Node-Exporter</a>                      | 9091       | Prometheus exporter for machine metrics                  | `prom/node-exporter:latest`    |
| <a href="#ouroboros">Ouroboros</a>  ----  Personally replaced with <a href="https://github.com/tamimology/docker-containers#watchtower">WatchTower</a>                               | -          | Update containers with latest images                     | `pyouroboros/ouroboros:latest` |
| <a href="#portainer-ce">Portainer-CE</a>  ----  Personally replaced with `portainer/portainer-ee:alpine`                        | 9000,9443,8000 | Docker management tool (FREE Community Edition)      | `portainer/portainer-ce:alpine` |
| <a href="#tls-socket-proxy">TLS-Socket-Proxy</a>  ----  Personally replaced with <a href="https://github.com/tamimology/docker-containers#socket-proxy">Docker Socket Proxy                | 2376       | A security-enhanced proxy for the Docker Socket with TLS | `sjawhar/docker-socket-proxy:latest` |
| <a href="#container-webtty">Container-WebTTY</a>                   | 8818,8090       | SContainer WebTTY terminal access                    | `wrfly/container-web-tty:0.1.10`       |
| <a href="#whatsupdocker">WhatsUpDocker</a>  ----  Personally replaced with <a href="https://github.com/tamimology/docker-containers#watchtower">WatchTower                      | 3000       | Same as WatchTower but with UI and actions               | `fmartinou/whats-up-docker:latest` |



# LINKS AND PAGE ORGANISATION

| Container                                                       | Port       | Description                                        | Docker Image                           |
| :------------                                                   | :----      | :-------                                           | :---                                   |
| <a href="#dashmachine">DashMachine</a>  ----  Personally replaced with <a href="https://github.com/tamimology/docker-containers#homarr">Homarr</a>                          | 5050       | Links manager                                      | `rmountjoy/dashmachine:latest`         |
| <a href="#shiori">Shiori</a>                                    | 18080      | A simple bookmarks manager                         | `ghcr.io/go-shiori/shiori:latest`      |



# MEDIA PLAYING

| Container                                                       | Port       | Description                                        | Docker Image                           |
| :------------                                                   | :----      | :-------                                           | :---                                   |
| <a href="#emby">Emby</a>  ----  Personally replaced with <a href="https://github.com/tamimology/docker-containers#jellyfin">Jellyfin</a>                                        | 8096       | Media server                                       | `emby/embyserver:latest`               |
| <a href="#embystat">EmbyStat</a>  ----  Personally replaced with <a href="https://github.com/tamimology/docker-containers#jellystat">JellyStat</a>                                | 6555       | Stat web interface for Emby Media server           | `uping/embystat:beta-linux-x64`        |
| <a href="#reiverr">Reiverr</a>                                  | 9494       | A single UI interacting with TMDB, Jellyfin, Radarr and Sonarr | `ghcr.io/aleksilassila/reiverr:latest` |
| <a href="#jellyseerr">Jellyseerr</a>                            | 6556       | Managing requests for media library                | `fallenbagel/jellyseerr:latest`        |
| <a href="#plex-media-server">Plex Media Server</a>  ----  Personally replaced with <a href="https://github.com/tamimology/docker-containers#jellyfin">Jellyfin</a>              | 32400      | Media server (standby)                             | `plexinc/pms-docker:latest`            |




# NETWORKING AND SECURITY

| Container                                                       | Port       | Description                                        | Docker Image                           |
| :------------                                                   | :----      | :-------                                           | :---                                   |
| <a href="#adguard">AdGuard</a>  ----  Personally replaced with pfSense                                 | 80, 3000        | Network-wide software for blocking ads & tracking    | `adguard/adguardhome:latest`           |
| <a href="#adguard">AdGuard-Sync</a>  ----  Personally replaced with pfSense                                 | 3006        | Synchronize AdGuardHome config to replica instances    | `ghcr.io/bakito/adguardhome-sync:latest`           |
| <a href="#cloudflare-ddns">CloudFlare DDNS</a>  ----  Personally replaced with <a href="https://github.com/tamimology/docker-containers#cloudflared">Cloudflared</a>                  | -          | Dynamic DNS updater for CloudFlare Tunnel          | `oznu/cloudflare-ddns:latest`          |
| <a href="#ddns-updater">DDNS Updater</a>  ----  Personally replaced with pfSense                           | 8002            | Dynamic DNS updater for mulit-DDNS services in one   | `qmcgaw/ddns-updater:latest`           |
| <a href="#dhcpdns">DHCPdns</a> - OBSELETE, ISC ceased maintaining ISC DHCP since 2022, replaced by <a href="https://git.tamimology.com/tam/docker-containers#user-content-kea">Kea</a>                                 | 10000,67   | A DHCPdns with Webmin GUI to assign static IPs     | `mayankt/dhcpdns:latest`               |
| <a href="#doku">Doku</a>                                        | 9090       | Monitor docker disk usage in a user-friendly manner | `amerkurev/doku:latest`               |
| <a href="#duckdns">DuckDNS</a>  ----  Personally replaced with <a href="https://github.com/tamimology/docker-containers#ddns-updater">DDNS Updater</a>                                   | -          | Dynamic DNS updater for DuckDNS                    | `maksimstojkovic/duckdns:latest`       |
| <a href="#fail2ban">Fail2Ban</a>                                | -          | Blockes IPs of Brute Force logins 'for Authelia'   | `crazymax/fail2ban:latest`             |
| <a href="#goaccess">GoAccess</a>                                | 7880       | Parse proxy logs for Nginx Proxy Manager           | `xavierh/goaccess-for-nginxproxymanager:latest` |
| <a href="#kea">Kea</a>  ----  Personally replaced with pfSense                                             | -               | A DHCP server to assign static IPs (no UI)           | `jonasal/kea-dhcp4:2`                  |
| <a href="#lets-encrypt">Lets Encrypt</a>  ----  Personally replaced with <a href="https://github.com/tamimology/docker-containers#cloudflared">Cloudflared</a>                        | -          | Auto HTTPS certificate generator from Let's Encrypt | `maksimstojkovic/letsencrypt:latest`  |
| <a href="#middlefinger">MiddleFinger</a>                        | 404        | Shows a dual 00100 hands for any unauthorised access | `modem7/middle-finger:dual`          |
| <a href="#snipeit">SnipeIT</a>                                  | 1339       | Open source IT asset management system             | `snipe/snipe-it:latest-alpine`         |
| <a href="#webdav">WebDAV</a>  ----  Personally replaced with <a href="https://github.com/tamimology/docker-containers#dufs">Dufs</a>                                    | 8888       | Access local files using WebDAV                     | `bytemark/webdav:latest`              |
| <a href="#webnut">webNUT</a>                                    | 6543       | UPS network monitoring web ui                       | `edgd1er/webnut:latest`               |



# PROGRAMMING

| Container                                                       | Port       | Description                                        | Docker Image                           |
| :------------                                                   | :----      | :-------                                           | :---                                   |
| <a href="#appdaemon">AppDaemon</a>                              | 6060       | Sandbox py-execution environment for HomeAssistant | `acockburn/appdaemon:latest`           |
| <a href="#it-tools">IT-Tools</a>                                | 5545       | Useful tools for developers working in IT          | `corentinth/it-tools:latest`           |
| <a href="#visual-studio-code ">Visual Studio Code </a>  ----  Personally replaced with <a href="https://github.com/tamimology/docker-containers#code-server">Code-Server</a>          | 8181       | Code editor                                        | `codercom/code-server:latest`          |



# SYSTEM MONITORING AND MANAGEMENT

| Container                                                       | Port       | Description                                        | Docker Image                           |
| :------------                                                   | :----      | :-------                                           | :---                                   |
| <a href="#home-panel">Home-Panel</a>                            | 8234       | Web frontend for controlling the home              | `timmo001/home-panel:latest`           |



# SELF-HOSTED

| Container                                                       | Port       | Description                                        | Docker Image                           |
| :------------                                                   | :----      | :-------                                           | :---                                   |
| <a href="#2fauth">2FAuth</a>                                    | 8010       | 2FAuth OTP Generator                               | `2fauth/2fauth:latest`                 |
| <a href="#chatvault">Chatvault</a>                              | 8787       | Stores backups of WhatsApp conversations from various sources | `ghcr.io/vitormarcal/chatvault:latest` |
| <a href="#collabora">Collabora</a>                              | 9988       | Editing documents in a browser from supported applications | `tiredofit/collabora-online:latest` |
| <a href="#dufs">Dufs</a>                                           | 4688            | Utility file server that supports webdav             | `sigoden/dufs:latest`                  |
| <a href="#flatnotes">FlatNotes</a>  ----  Personally replaced with <a href="https://github.com/tamimology/docker-containers#wiznote">WizNote</a>                              | 8053       | A database-less note-taking web app                | `dullage/flatnotes:latest`             |
| <a href="#forgejo">Forgejo</a>  ----  Personally replaced with <a href="https://github.com/tamimology/docker-containers#gitea">Gitea</a>                                | 3332,223   | A self-hosted lightweight software forge           | `codeberg.org/forgejo/forgejo:1.21`    |
| <a href="#joplin">Joplin</a>  ----  Personally replaced with <a href="https://github.com/tamimology/docker-containers#wiznote">WizNote</a>                                    | 2300       | Open source note taking and to-do application      | `joplin/server:latest`                 |
| <a href="#kanboard">KanBoard</a>  ----  Personally replaced with <a href="https://github.com/tamimology/docker-containers#focalboard">FocalBoard</a>                                 | 4480,4443  | Kanban Board                                       | `kanboard/kanboard:latest`             |
| <a href="#librex">LibreX</a>  ----  Personally replaced with <a href="https://github.com/tamimology/docker-containers#librey">LibreY</a>                                     | 8245       | Free privacy respecting meta search engine Google Alternative | `librex/librex:latest`      |
| <a href="#libretranslate">LibreTranslate</a>                    | 7821       | Open Source Machine Translation API | `libretranslate/libretranslate:latest`      |
| <a href="#lingva-translate">Lingva-Translate</a>                | 6455       | Alternative front-end for Google Translate  | `thedaviddelta/lingva-translate:latest`      |
| <a href="#memos">Memos</a>  ----  Personally replaced with <a href="https://github.com/tamimology/docker-containers#wiznote">WizNote</a>                                         | 5230            | Capture and share your great thoughts                | `neosmemo/memos:latest`                |
| <a href="#our-shopping-list">Our Shopping List</a>              | 8989       | A simple shared shopping list application          | `nanawel/our-shopping-list:latest`     |
| <a href="#photoprism">Photoprism</a>  ----  Personally replaced with <a href="https://github.com/tamimology/docker-containers#immich">Immich</a>                            | 2342       | AI-Powered Photos App for the Decentralized Web    | `photoprism/photoprism:latest`         |
| <a href="#photoview">Photoview</a>  ----  Personally replaced with <a href="https://github.com/tamimology/docker-containers#immich">Immich</a>                              | 7354       | A simple and user-friendly Photo Gallery           | `viktorstrate/photoview:2`             |
| <a href="#pigallery">Pigallery</a>  ----  Personally replaced with <a href="https://github.com/tamimology/docker-containers#immich">Immich</a>                              | 9842       | A self-hosted directory-first photo gallery website | `bpatrik/pigallery2:latest`           |
| <a href="#picoshare">PicoShare</a>                              | 8179       | Minimalist service to share files easily           | `mtlynch/picoshare:latest`             |
| <a href="#piped-backend">Piped-Backend</a>  ----  Personally replaced with <a href="https://github.com/tamimology/docker-containers#invidious">Invidious</a>                      | -          | An advanced open-source privacy alternative to YouTube | `1337kavin/piped:latest`           |
| <a href="#piped-frontend">Piped-Frontend</a>  ----  Personally replaced with <a href="https://github.com/tamimology/docker-containers#invidious">Invidious</a>                    | -          | A front UI for Piped-Backeend                      | `1337kavin/piped-frontend:latest`      |
| <a href="#piped-nginx">Piped-Nginx</a>  ----  Personally replaced with <a href="https://github.com/tamimology/docker-containers#invidious">Invidious</a>                          | 7601       | Redirects all Youtube to Piped-Backend             | `nginx:mainline-alpine`                |
| <a href="#piped-proxy">Piped-Proxy</a>  ----  Personally replaced with <a href="https://github.com/tamimology/docker-containers#invidious">Invidious</a>                          | -          | A proxy for Piped-Backend                          | `1337kavin/piped-proxy:latest`         |
| <a href="#piwigo">Piwigo</a>  ----  Personally replaced with <a href="https://github.com/tamimology/docker-containers#immich">Immich</a>                                    | 8100       | Photos/videos managing with face recognition       | `ghcr.io/linuxserver/piwigo:latest`    |
| <a href="#searxng">SearXNG</a>  ----  Personally replaced with <a href="https://github.com/tamimology/docker-containers#librey">LibreY</a>                                  | 5080       | Privacy-respecting, hackable metasearch engine     | `searxng/searxng:latest`               |
| <a href="#vault">Vault</a>                                      | 8205       | A tool for secrets management                      | `hashicorp/vault:latest`               |
| <a href="#xbackbone">xBackbone</a>  ----  Personally replaced with <a href="https://github.com/tamimology/docker-containers#projectsend">ProjectSend</a>                              | 8522,4443  | A lightweight file manager with full ShareX support | `lscr.io/linuxserver/xbackbone:latest` |



To begin with, I defined my own networks to use, that makes my setup easier for IP allocation of some containers.

```
version: '3'

networks:

# Bridge Network
  my_bridge:
    name: my_bridge
    driver: bridge
    attachable: true
    ipam:
     config:
       - subnet: $BRIDGE_SUBNET
         gateway: $BRIDGE_GATEWAY

# MacVLAN Network
  dockervlan:
    name: dockervlan
    driver: macvlan
    driver_opts:
      parent: $ETHCARD
      macvlan_mode: bridge
    ipam:
      config:
        - subnet: "$SUBNET"
          gateway: "$MACVLAN_GATEWAY"
          ip_range: "$MACVLAN_RANGE"

services:
```


Now assuming that every `docker-compose` file you use, will have the standard parameters on top, then you just paste each compose below that corresponds to your need. I have included those in the network definition above.

You can add them to each file separately if needed. I assume that you will be using 1 file for all of the containers, and then `docker-compose up -d CONTAINER_NAME` to start every single container. You can add as many CONTAINER_NAME as needed i.e.

`docker-compose up -d cadvisor` will start the cAdvisor container only. If you use `docker-compose up -d` then all containers in the compose file will be started in one command.

```
version: '3'

services:
```

# 2FAuth

<details>
  <summary>
  </summary>

```
  2fauth:
    container_name: 2fauth
    restart: $RESTART_POLICY
    hostname: 2fauth
    environment:
      - APP_NAME=My Server 2FAuth
      - APP_ENV=local
      - APP_DEBUG=false
      - SITE_OWNER=$GM_USER
      - APP_KEY=$APP_KEY
      - APP_URL=https://2fauth.$DOMAINNAME
      - IS_DEMO_APP=false
      - LOG_CHANNEL=daily
      - APP_LOG_LEVEL=notice # debug, info, notice, warning, error, critical, alert, emergency
      - DB_CONNECTION=sqlite #can only be sqlite
      - DB_DATABASE="/srv/database/database.sqlite"
      # If you're looking for performance improvements, you could install memcached.
      - CACHE_DRIVER=file
      - SESSION_DRIVER=file
      - FILESYSTEM_DRIVER=local
      - MAIL_DRIVER=log
      - MAIL_HOST=$GM_HOST
      - MAIL_PORT=$GM_PORT
      - MAIL_FROM=$GM_USER
      - MAIL_USERNAME=$GM_USER
      - MAIL_PASSWORD=$GM_PSW
      - MAIL_ENCRYPTION=null
      - MAIL_FROM_NAME=2FAuth
      - MAIL_FROM_ADDRESS=$GM_USER
      - AUTHENTICATION_GUARD=web-guard
      - AUTH_PROXY_HEADER_FOR_USER=null
      - AUTH_PROXY_HEADER_FOR_EMAIL=null
      - WEBAUTHN_NAME=My Server 2FAuth
      - WEBAUTHN_ID=null
      - WEBAUTHN_ICON=null
      - WEBAUTHN_USER_VERIFICATION=preferred
      - TRUSTED_PROXIES=null  # * to trust any proxy, or a comma-separated IP list
      - BROADCAST_DRIVER=log
      - QUEUE_DRIVER=sync
      - SESSION_LIFETIME=12
      - REDIS_HOST=2fauth-redis
      - REDIS_PASSWORD=null
      - REDIS_PORT=6378
      - MIX_ENV=local
      #- PUSHER_APP_ID=
      #- PUSHER_APP_KEY=
      #- PUSHER_APP_SECRET=
      #- PUSHER_APP_CLUSTER=mt1
      #- MIX_PUSHER_APP_KEY="${PUSHER_APP_KEY}"
      #- MIX_PUSHER_APP_CLUSTER="${PUSHER_APP_CLUSTER}"
    volumes:
      - $PERSIST/2fauth:/2fauth
    ports:
      - 8010:8000/tcp
    networks:
      my_bridge:
    depends_on:
      - 2fauth-redis
    image: '2fauth/2fauth:3.0.2' #latest'
```
</details>

[🔼 Back to top](#self-hosted)


# 2FAuth-Redis

<details>
  <summary>
  </summary>

```
  2fauth-redis:
    container_name: 2fauth-redis
    restart: $RESTART_POLICY
    hostname: 2fauth-redis
    environment:
      - TZ=$TZ
    volumes:
      - $PERSIST/2fauth/redis:/data
    expose:
      - 6378
    networks:
      my_bridge:
    image: 'redis:alpine'
```
</details>

[🔼 Back to top](#databases)


# AdGuard

<details>
  <summary>
  </summary>

```
  adguard:
    container_name: adguard
    restart: $ALWAYS_ON_POLICY
    hostname: adguard
    environment:
      - TZ=$TZ
    volumes:
      - $PERSIST/adguard/conf:/opt/adguardhome/conf 
      - $PERSIST/adguard/work:/opt/adguardhome/work 
      - $PERSIST/adguard/certs:/certs
    ports:
      - 53/udp 
      - 67/udp 
      - 68/tcp 
      - 68/udp 
      - 80/tcp 
      - 443/tcp 
      - 853/tcp 
      - 3000/tcp 
    networks:
      my_bridge:
         ipv4_address: $BRIDGE_NET.202
      dockervlan:
        ipv4_address: $MACVLAN_NET.5 # IP address inside defined $MACVLAN_RANGE range
    security_opt:
      - no-new-privileges:true
    labels: 
      autoheal: $AUTOHEAL_RESTART
      monocker.enable: $MONOCKER_ENABLE
    image: 'adguard/adguardhome:latest'
```
</details>

[🔼 Back to top](#networking-and-security)


# AdGuard-Sync

<details>
  <summary>
  </summary>

```
  adguard-sync:
    container_name: adguard-sync
    restart: $ALWAYS_ON_POLICY
    hostname: adguard-sync
    command: run --config /config/adguardhome-sync.yaml
    environment:
      - LOG_LEVEL=info
      - PUID=$PUID
      - PGID=$PGID
      - TZ=$TZ
    volumes:
      - $PERSIST/adguard/sync/adguardhome-sync.yaml:/config/adguardhome-sync.yaml
    networks:
      my_bridge:
      dockervlan:
        ipv4_address: $MACVLAN_NET.7 #must be in the same macvlan as original instance
    labels: 
      autoheal: $AUTOHEAL_RESTART
      monocker.enable: $MONOCKER_ENABLE
    ports:
      - 3006:8080
    image: 'ghcr.io/bakito/adguardhome-sync:latest'
```
</details>

[🔼 Back to top](#networking-and-security)


# AlertManager
<details>
  <summary>
  </summary>

```
  alertmanager:
    container_name: alertmanager
    restart: $ALWAYS_ON_POLICY
    hostname: alertmanager
    volumes:
      - $PERSIST/alertmanager/:/etc/alertmanager/
    ports:
      - 9093:9093
    networks:
      my_bridge:
    command:
      - '--config.file=/etc/alertmanager/config.yml'
      - '--storage.path=/alertmanager'
    image: 'prom/alertmanager:latest'
```
</details>

[🔼 Back to top](#docker-related)


# AndroidTV ADB
<details>
  <summary>
  </summary>

```
  androidtv-adb:
    container_name: androidtv-adb
    restart: $ALWAYS_ON_POLICY
    hostname: androidtv-adb
    volumes:
     - $PERSIST/adb/config:/config
     - $LOCAL_TIME:/etc/localtime:ro
    ports:
     - 5037:5037
    networks:
      my_bridge:
    command: sh -c "/config/startup.sh & adb -a -P 5037 server nodaemon"
    image: 'sorccu/adb:latest'
```
</details>

[🔼 Back to top](#networking-and-security)


# AppDaemon
<details>
  <summary>
  </summary>

```
  appdaemon:
    container_name: appdaemon
    restart: $ALWAYS_ON_POLICY
    hostname: appdaemon
    environment:
        - HA_URL=http://$LOCAL_HOST:8123
        - TOKEN=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiIxNjY2ZWMwZGI4YWE0MTYwODQ5NDYzMWEyNWY5ODAzMyIsImlhdCI6MTY4ODQ1ODUyMSwiZXhwIjoyMDAzODE4NTIxfQ.PEsdgMBC0Oc2Zl9Wl2VYPY2LGKzqvO02XxkkeQSnfL8
        - DASH_URL=http://$LOCAL_HOST:6060
    volumes:
        - $PERSIST/appdaemon:/conf'
    ports:
        - 6060:5050
    networks:
      my_bridge:
    image: 'acockburn/appdaemon:latest'
```
</details>

[🔼 Back to top](#programming)


# Auto-Heal
<details>
  <summary>
  </summary>

```
  autoheal:
    container_name: autoheal
    restart: $ALWAYS_ON_POLICY
    hostname: autoheal
    environment:
      - AUTOHEAL_CONTAINER_LABEL=autoheal #all
      - DOCKER_SOCK=$DOCKER_HOST
      - APPRISE_URL=http://$LOCAL_HOST:8001/notify/autoheal
      - AUTOHEAL_INTERVAL=5   # check every 5 seconds
      - AUTOHEAL_START_PERIOD=600   # wait 10 minutes before the first health check
      - AUTOHEAL_DEFAULT_STOP_TIMEOUT=60   # Docker waits max 10 seconds (the Docker default) for a container to stop before killing during restarts (container overridable via label, see below)
      - CURL_TIMEOUT=30     # --max-time seconds for curl requests to Docker API
      # - POST_RESTART_SCRIPT=""    # Run the specified script if a container was re
    volumes:
      - $LOCAL_TIME:/etc/localtime:ro
    networks:
      my_bridge:
    image: 'modem7/docker-autoheal:latest'
```
</details>

[🔼 Back to top](#docker-related)


# BroadLinkManager
<details>
  <summary>
  </summary>

```
  broadlinkmanager:
    container_name: broadlinkmanager
    restart: $RESTART_POLICY
    hostname: broadlinkmanager
    environment:
      - ENABLE_GOOGLE_ANALYTICS=False #Optional, default is True, Set to False if you want to disable Google Analytics
    volumes:
      - $PERSIST/broadlinkmanager:/opt/broadlinkmanager/data
    ports:
      - 7020:7020
    network_mode: host
    image: 'techblog/broadlinkmanager:latest'
```
</details>

[🔼 Back to top](#docker-related)


# cAdvisor
<details>
  <summary>
  </summary>

```
  cadvisor:
    container_name: cadvisor
    restart: $RESTART_POLICY
    hostname: cadvisor
    volumes:
      - /:/rootfs:ro
      - /var/run:/var/run:ro
      - /volume1/@docker:/var/lib/docker:ro # USE IN SSH TO GET IT ( docker info | grep "Docker Root Dir:" )
      - /dev:/dev:ro

      - /sys/fs/cgroup/cpu:/sys/fs/cgroup/cpu:ro

      # - /sys/fs/cgroup/cpuset:/sys/fs/cgroup/cpuset:ro

      - /sys/fs/cgroup/cpuacct/docker:/sys/fs/cgroup/cpuacct:ro
      - /sys/fs/cgroup/cpuacct/cgroup.clone_children:/sys/fs/cgroup/cpuacct/cgroup.clone_children:ro
      - /sys/fs/cgroup/cpuacct/cgroup.event_control:/sys/fs/cgroup/cpuacct/cgroup.event_control:ro
      - /sys/fs/cgroup/cpuacct/cgroup.procs:/sys/fs/cgroup/cpuacct/cgroup.procs:ro
      - /sys/fs/cgroup/cpuacct/cpuacct.stat:/sys/fs/cgroup/cpuacct/cpuacct.stat:ro
      - /sys/fs/cgroup/cpuacct/cpuacct.usage:/sys/fs/cgroup/cpuacct/cpuacct.usage:ro
      - /sys/fs/cgroup/cpuacct/cpuacct.usage_percpu:/sys/fs/cgroup/cpuacct/cpuacct.usage_percpu:ro
      - /sys/fs/cgroup/cpuacct/notify_on_release:/sys/fs/cgroup/cpuacct/notify_on_release:ro
      - /sys/fs/cgroup/cpuacct/tasks:/sys/fs/cgroup/cpuacct/tasks:ro
    privileged: true
    devices:
        - /dev/kmsg
    ports:
      - 8888:8080
    networks:
      my_bridge:        
    image: 'gcr.io/cadvisor/cadvisor:latest'
```
</details>

[🔼 Back to top](#docker-related)


# ChatVault
<details>
  <summary>
  </summary>

```
  chatvault:
    container_name: chatvault
    restart: $RESTART_POLICY
    hostname: chatvault
    environment:
      - SPRING_DATASOURCE_URL=jdbc:postgresql://chatvault-postgres:5432/chatvaultdb
      - SPRING_DATASOURCE_USERNAME=chatvault
      - SPRING_DATASOURCE_PASSWORD=$DB_PASSWORD
      - CHATVAULT_MSGPARSER_DATEFORMAT="dd/MM/yyyy HH:mm"
      - APP_EMAIL_ENABLED=true
      - APP_EMAIL_HOST=$GM_HOST
      - APP_EMAIL_PASSWORD=$GM_PSW
      - APP_EMAIL_PORT=$GM_PORT
      - APP_EMAIL_USERNAME=$GM_USER
    volumes:
      - $PERSIST/chatvault:/opt/chatvault
      - $PERSIST/chatvault/config:/config
    networks:
      my_bridge:
    ports:
      - 8787:8080
    depends_on:
      - chatvault-postgres
    image: 'ghcr.io/vitormarcal/chatvault:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# ChatVault-Postgres
<details>
  <summary>
  </summary>

```
  chatvault-postgres:
    container_name: chatvault-postgres
    restart: $RESTART_POLICY
    hostname: chatvault-postgres
    volumes:
      - $PERSIST/chatvault/db:/var/lib/postgresql/data
    environment:
      TZ: $TZ
      POSTGRES_DB: chatvaultdb
      POSTGRES_USER: chatvault
      POSTGRES_PASSWORD: $DB_PASSWORD
    networks:
      my_bridge:
    security_opt:
      - no-new-privileges:true
    user: $PUID:$PGID
    healthcheck:
      test: ["CMD", "pg_isready", "-q", "-d", "chatvaultdb", "-U", "chatvault"]
      timeout: 45s
      interval: 10s
      retries: 10
    labels: 
      autoheal: $AUTOHEAL_RESTART
    image: 'postgres:alpine'
```
</details>

[🔼 Back to top](#databases)


# CheckMK
<details>
  <summary>
  </summary>

```
  checkmk:
    container_name: checkmk
    restart: $ALWAYS_ON_POLICY
    hostname: checkmk
    volumes:
      - $PERSIST/checkmk:/omd/sites
      - $LOCAL_TIME:/etc/localtime:ro
    ports:
      - 8722:5000
    networks:
      my_bridge:
    tmpfs: /opt/omd/sites/cmk/tmp:uid=1000,gid=1000
    image: 'checkmk/check-mk-raw:latest'
```
</details>

[🔼 Back to top](#docker-related)


# CloudFlare-DDNS
<details>
  <summary>
  </summary>

```
  cloudflare-ddns:
    container_name: cloudflare-ddns
    restart: $ALWAYS_ON_POLICY
    hostname: cloudflare-ddns
    environment:
      - API_KEY=$DDNS_AUTOUPDATE_TOKEN
      - ZONE=tamimology.com
      - PROXIED=true
      - PUID=$PUID
      - PGID=$PGID
    networks:
      my_bridge:
    image: 'oznu/cloudflare-ddns:latest'
```
</details>

[🔼 Back to top](#networking-and-security)


# Collabora
<details>
  <summary>
  </summary>

```
  collabora:
    image: 'tiredofit/collabora-online:latest'
    container_name: collabora
    # hostname: online.example.com
    cap_add:
      - MKNOD
      - NET_ADMIN
    privileged: true
    # volumes:
    #   - $PERSIST/logs:/logs
    environment:
      - TIMEZONE=$TZ
      - CONTAINER_NAME=collabora
      - ADMIN_USER=admin
      - ADMIN_PASS=collabora-online
      - ALLOWED_HOSTS=localhost
      - ENABLE_TLS=FALSE
      # - ENABLE_TLS_REVERSE_PROXY=TRUE
      - INTERFACE=notebookbar
      - LOG_TYPE=FILE
    ports:
      - 9988:9980
    networks:
      my_bridge:
    restart: always
```
</details>

[🔼 Back to top](#self-hosted)


# Container-Mon
<details>
  <summary>
  </summary>

```
  container-mon:
    container_name: container-mon
    restart: $RESTART_POLICY
    hostname: container-mon
    # volumes:
    #   - /var/run/docker.sock:/var/run/docker.sock
    environment:
      - DOCKER_HOST=$DOCKER_HOST
      - CONTAINERMON_CRON=*/1 * * * * # every 1 min
      - CONTAINERMON_FAIL_LIMIT=1
      - CONTAINERMON_USE_LABELS=false #If true will only monitor containers with the label containermon.enable=true set
      - CONTAINERMON_NOTIFICATION_URL=pushover://shoutrrr:$PUSHOVER_DOCKERHEALTH_API@$PUSHOVER_USER_KEY
      - CONTAINERMON_NOTIFY_HEALTHY=true
      - CONTAINERMON_CHECK_STOPPED=false
    networks:
      my_bridge:
    image: 'ghcr.io/rafhaanshah/container-mon:latest'
```
</details>

[🔼 Back to top](#docker-related)


# DashMachine
<details>
  <summary>
  </summary>

```
  dashmachine:
    container_name: dashachine
    restart: $RESTART_POLICY
    hostname: dashmachine
    volumes:
      - $PERSIST/dashmachine:/dashmachine/dashmachine/user_data
    ports:
      - 5050:5000
    networks:
      my_bridge:
    image: 'rmountjoy/dashmachine:latest'
```
</details>

[🔼 Back to top](#links-and-page-organisation)


# DeCompose
<details>
  <summary>
  </summary>

```
  decompose:
    container_name: decompose
    restart: $RESTART_POLICY
    hostname: decompose
    environment:
      DOCKER_HOST: $DOCKER_HOST
    # volumes:
    #   - /var/run/docker.sock:/var/run/docker.sock
    ports:
      - 4321:8080
    networks:
      my_bridge:
    image: 'techblog/decompose:latest'
```
</details>

[🔼 Back to top](#docker-related)


# DeUnHealth
<details>
  <summary>
  </summary>

```
  deunhealth:
    container_name: deunhealth
    restart: $RESTART_POLICY
    hostname: deunhealth
    environment:
      - DOCKER_HOST=unix://$DOCKER_SOCKET
      - LOG_LEVEL=info
      - HEALTH_SERVER_ADDRESS=127.0.0.1:9999
      - TZ=$TZ
    volumes:
      - $DOCKER_SOCKET:/var/run/docker.sock
    network_mode: "none"
    image: 'qmcgaw/deunhealth:latest'
```
</details>

[🔼 Back to top](#docker-related)


# DDNS-Updater
<details>
  <summary>
  </summary>

```
  ddns-updater:
    container_name: ddns-updater
    restart: $ALWAYS_ON_POLICY
    hostname: ddns-updater
    environment:
      - TZ=$TZ
      - PUID=$PUID
      - PGID=$PGID
      # - USER="0" #run the container as root 
      - PERIOD=5m
      - UPDATE_COOLDOWN_PERIOD=5m
      - PUBLICIP_FETCHERS=all
      - PUBLICIP_HTTP_PROVIDERS=all
      - PUBLICIPV4_HTTP_PROVIDERS=all
      - PUBLICIPV6_HTTP_PROVIDERS=all
      - PUBLICIP_DNS_PROVIDERS=all
      - PUBLICIP_DNS_TIMEOUT=3s
      - HTTP_TIMEOUT=10s
      - LISTENING_PORT=8000
      - HEALTH_SERVER_ADDRESS=127.0.0.1:9999
      - ROOT_URL=/
      - BACKUP_PERIOD=24h # 0 to disable
      - BACKUP_DIRECTORY=/updater/data
      - LOG_LEVEL=info
      - LOG_CALLER=hidden
      - SHOUTRRR_ADDRESSES=pushover://shoutrrr:$PUSHOVER_DDNS_API@$PUSHOVER_USER_KEY
    volumes:
      - $PERSIST/ddns-updater:/updater/data
    networks:
      my_bridge:
    ports:
      - 8002:8000/tcp
    user: $PUID:$PGID
    labels: 
      autoheal: $AUTOHEAL_RESTART
      autoheal.stop.timeout: 30
      monocker.enable: $MONOCKER_ENABLE
    image: 'qmcgaw/ddns-updater:latest'
```
</details>

[🔼 Back to top](#networking-and-security)


# DHCPdns
<details>
  <summary>
  </summary>

```
  dhcpdns:
    container_name: dhcpdns
    restart: $ALWAYS_ON_POLICY
    hostname: dhcpdns
    privileged: true
    environment:
      - PUID=$PUID
      - PGID=$PGID
    volumes:
        - $PERSIST/dhcpdns:/data
        - $PERSIST/dhcpdns/dhcp/dhcpd.conf:/data/dhcp/dhcpd.conf:rw
    network_mode: host
    ports:
      - 67:67/udp
      - 10000:10000/tcp
    cap_add:
        - NET_ADMIN
    image: 'mayankt/dhcpdns:latest'
```
</details>

[🔼 Back to top](#networking-and-security)


# DockGE
<details>
  <summary>
  </summary>

```
  dockge:
    container_name: dockge
    restart: $RESTART_POLICY
    hostname: dockge
    environment:
      - DOCKER_HOST=$DOCKER_HOST #$SOCKET
      - DOCKGE_STACKS_DIR=$PERSIST/dockge/stacks
    volumes:
      # - $DOCKER_SOCKET:/var/run/docker.sock
      - $PERSIST/dockge/data:/app/data
      - $PERSIST/dockge/stacks:$PERSIST/dockge/stacks #(Both paths match)
    ports:
      - 6001:5001
    networks:
      my_bridge:
    image: 'louislam/dockge:1'
```
</details>

[🔼 Back to top](#docker-related)


# Doku
<details>
  <summary>
  </summary>

```
  doku:
    container_name: doku
    restart: $RESTART_POLICY
    hostname: doku
    environment:
        - DOCKER_HOST=$DOCKER_HOST
    volumes:
        # - $DOCKER_SOCKET:/var/run/docker.sock:ro
        - /:/hostroot:ro
    ports:
        - 9090:9090
    image: 'amerkurev/doku:latest'
```
</details>

[🔼 Back to top](#networking-and-security)


# DuckDNS
<details>
  <summary>
  </summary>

```
  duckdns:
    container_name: duckdns
    restart: $ALWAYS_ON_POLICY
    hostname: duckdns
    environment:
      - DUCKDNS_DOMAIN=$DOMAINNAME 
      - DUCKDNS_TOKEN=$DUCKTOKEN 
      - DUCKDNS_DELAY=5
      - TZ=$TZ
    networks:
      my_bridge:
    image: 'maksimstojkovic/duckdns:latest'
```
</details>

[🔼 Back to top](#networking-and-security)


# Dufs
<details>
  <summary>
  </summary>

```
  dufs:
    container_name: dufs
    restart: $RESTART_POLICY
    hostname: dufs
    volumes:
      - $PERSIST/dufs:/data
    networks:
      my_bridge:
    ports:
      - 4688:5000
    command: /data -A
    tty: true
    image: 'sigoden/dufs:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# Emby
<details>
  <summary>
  </summary>

```
  emby:
    container_name: emby
    restart: $ALWAYS_ON_POLICY
    hostname: emby
    environment:
      - UID=1028
      - GID=2
      - TZ=$TZ
    volumes:
      - $PERSIST/emby/config:/config:rw
      - $PERSIST/emby/backup:/backup:rw
      - $MEDIA_PATH:/media:rw
      # - /dev/shm:/data/transcode
    ports:
      - 0.0.0.0::1900/udp
      - 0.0.0.0::7359/udp
      - 8096:8096/tcp
      - 2096:8920/tcp #cloudflare supported https port https://developers.cloudflare.com/fundamentals/get-started/reference/network-ports/
    networks:
      my_bridge:
    # devices:
    #   - /dev/dri:/dev/dri # for hardware transcoding
    labels: 
      monocker.enable: $MONOCKER_ENABLE
    image: 'emby/embyserver:latest'
```
</details>

[🔼 Back to top](#media-playing)


# EmbyStat
<details>
  <summary>
  </summary>

```
  embystat:
    container_name: embystat
    restart: $RESTART_POLICY
    hostname: embystat
    environment:
      - PUID=$PUID
      - PGID=$PGID
      - TZ=$TZ
    volumes:
      - $PERSIST/embystat:/app/config
    ports:
      - 6555:6555
    networks:
      my_bridge:
    image: 'uping/embystat:beta-linux-x64'
```
</details>

[🔼 Back to top](#media-playing)


# Fail2Ban
<details>
  <summary>
  </summary>

```
  fail2ban:
    container_name: fail2ban
    restart: $RESTART_POLICY
    hostname: fail2ban
    environment: 
      - PUID=$PUID
      - PGID=$PGID
      - TZ=$TZ
      - F2B_DB_PURGE_AGE=30d
      - F2B_LOG_TARGET=/data/fail2ban.log
      - F2B_LOG_LEVEL=INFO
      - F2B_IPTABLES_CHAIN=INPUT
      # - SSMTP_HOST=$GM_HOST
      # - SSMTP_PORT=$GM_PORT
      # - SSMTP_HOSTNAME=pop.gmail.com
      # - SSMTP_USER=$GM_USER
      # - SSMTP_PASSWORD=$GM_PSW
      # - SSMTP_TLS=NO
      # - SSMTP_STARTTLS=NO
    volumes:
      - $PERSIST/fail2ban:/data
      - $PERSIST/authelia/config:/var/log/authelia:ro
      - $PERSIST/vaultwarden/vaultwarden.log:/var/log/vaultwarden/vaultwarden.log:ro
      - $PERSIST/data/navidrome/navidrome.log:/var/log/navidrome/navidrome.log:ro
    network_mode: host
    privileged: true
    cap_add:
      - NET_ADMIN
      - NET_RAW
    image: 'crazymax/fail2ban:latest'
```
</details>

[🔼 Back to top](#networking-and-security)


# FlatNotes
<details>
  <summary>
  </summary>

```
  flatnotes:
    container_name: flatnotes
    restart: $RESTART_POLICY
    hostname: flatnotes
    environment:
      PUID: $PUID
      PGID: $PGID
      FLATNOTES_AUTH_TYPE: "password"
      FLATNOTES_USERNAME: "admin"
      FLATNOTES_PASSWORD: "$DB_PASSWORD"
      FLATNOTES_SECRET_KEY: "$APP_KEY"
    volumes:
      - $PERSIST/flatnotes/data:/data
      - $PERSIST/flatnotes/index:/data/.flatnotes # Optional. Allows you to save the search index in a different location: 
    ports:
      - 8053:8080
    networks:
      my_bridge:
    image: 'dullage/flatnotes:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# Forgejo
<details>
  <summary>
  </summary>

```
  forgejo:
    container_name: forgejo
    restart: $RESTART_POLICY
    hostname: forgejo
    environment:
      - USER_UID=$PUID
      - USER_GID=$PGID
    networks:
      my_bridge:
    volumes:
      - $PERSIST/forgejo:/data
      - $TIME_ZONE:/etc/TZ:ro
      - $LOCAL_TIME:/etc/localtime:ro
    ports:
      - 3332:3000
      - 223:22
    image: 'codeberg.org/forgejo/forgejo:1.21'
```
</details>

[🔼 Back to top](#self-hosted)


# Glances
<details>
  <summary>
  </summary>

```
  glances:
    container_name: glances
    restart: $RESTART_POLICY
    hostname: glances
    environment:
      - TZ=$TZ
      - GLANCES_OPT=--webserver
    volumes:
      - $DOCKER_SOCKET:/var/run/docker.sock:ro
      - $PERSIST/glances:/glances/conf
    pid: host
    ports:
      - 61208:61208
    image: 'joweisberg/glances:latest'
```
</details>

[🔼 Back to top](#docker-related)


# GoAccess
<details>
  <summary>
  </summary>

```
  goaccess:
    container_name: goaccess
    restart: $RESTART_POLICY
    hostname: goaccess
    environment:
        - PUID=$PUID
        - PGID=$PGID
        - TZ=$TZ
        - BASIC_AUTH=true
        - BASIC_AUTH_USERNAME=admin
        - BASIC_AUTH_PASSWORD=$DB_PASSWORD
    volumes:
        - $PERSIST/nginx-proxy/logs:/opt/log
    ports:
        - 7880:7880
    networks:
      my_bridge:
    image: 'xavierh/goaccess-for-nginxproxymanager:latest'
```
</details>

[🔼 Back to top](#networking-and-security)


# Grafana
<details>
  <summary>
  </summary>

```
  grafana:
    container_name: grafana
    restart: $RESTART_POLICY
    hostname: grafana
    environment: #You can do this with any of the configuration options in https://github.com/grafana/grafana/blob/main/conf/defaults.ini by setting GF_<SectionName>_<KeyName>__FILE to the path of the file holding the secret.
      - GF_AUTH_DISABLE_LOGIN_FORM=true
      - GF_AUTH_ANONYMOUS_ENABLED=true
      - GF_AUTH_ANONYMOUS_ORG_ROLE=Admin
      - GF_SECURITY_ALLOW_EMBEDDING=true
      - GF_FEATURE_TOGGLES_ENABLE=flameGraph
      - GF_DASHBOARDS_MIN_REFRESH_INTERVAL=1s
      # - GF_AUTH_DISABLE_LOGIN_FORM=true
      # - GF_AUTH_ANONYMOUS_ENABLED=true
      # - GF_AUTH_ANONYMOUS_ORG_ROLE=Admin
      # - GF_SERVER_ROOT_URL
    user: '1000'    
    volumes:
      - $PERSIST/grafana:/var/lib/grafana
    ports:
      - 3003:3000
    networks:
      my_bridge:
    depends_on:
      - influxdb
    image: 'grafana/grafana:main'
```
</details>

[🔼 Back to top](#databases)


# Home-Panel
<details>
  <summary>
  </summary>

```
  homepanel:
    container_name: homepanel
    restart: $RESTART_POLICY
    hostname: homepanel
    volumes:
      - $PERSIST/homepanel/data:/data
    ports:
      - 8234:8234
    networks:
      my_bridge:
    image: 'timmo001/home-panel:latest'
```
</details>

[🔼 Back to top](#system-monitoring-and-management)


# IT-Tools
<details>
  <summary>
  </summary>

```
  it-tools:
    container_name: it-tools
    restart: $RESTART_POLICY
    hostname: it-tools
    networks:
      my_bridge:
    ports:
      - 5545:80
    image: 'corentinth/it-tools:latest'
```
</details>

[🔼 Back to top](#programming)


# Jellyseerr
<details>
  <summary>
  </summary>

```
  jellyseerr:
    container_name: jellyseerr
    restart: $RESTART_POLICY
    hostname: mediatracker
    environment:
        - LOG_LEVEL=debug
        - TZ=$TZ
    ports:
        - 6556:5055
    networks:
      my_bridge:
    volumes:
        - $PERSIST/jellyseerr:/app/config
    image: 'fallenbagel/jellyseerr:latest'
```
</details>

[🔼 Back to top](#media-playing)


# Joplin
<details>
  <summary>
  </summary>

```
  joplin:
    container_name: joplin
    restart: $RESTART_POLICY
    hostname: joplin
    environment:
        - APP_PORT=2230
        - APP_BASE_URL=https://joplin.$DOMAINNAME/
        - DB_CLIENT=pg
        - POSTGRES_PASSWORD=$DB_PASSWORD
        - POSTGRES_DATABASE=joplindb
        - POSTGRES_USER=joplin
        - POSTGRES_PORT=5432
        - POSTGRES_HOST=joplindb
    ports:
        - 2230:2230
    networks:
      my_bridge:
    depends_on:
        - joplindb
    image: 'joplin/server:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# JoplinDB
<details>
  <summary>
  </summary>

```
  joplindb:
    container_name: joplindb
    restart: $RESTART_POLICY
    hostname: joplindb
    environment:
        - POSTGRES_PASSWORD=$DB_PASSWORD
        - POSTGRES_USER=joplin
        - POSTGRES_DB=joplindb
    volumes:
        - $PERSIST/joplin:/var/lib/postgresql/data
    networks:
      my_bridge:
    image: 'postgres:13.1'
```
</details>

[🔼 Back to top](#databases)


# KanBoard
<details>
  <summary>
  </summary>

```
  kanboard:
    container_name: kanboard
    restart: $RESTART_POLICY
    hostname: kanboard
    volumes:
      - $PERSIST/kanboard/data:/var/www/app/data
      - $PERSIST/kanboard/plugins:/var/www/app/plugins
      - $PERSIST/kanboard/ssl:/etc/nginx/ssl
    # environment:
    #   DATABASE_URL: mysql://kanboard:$DB_PASSWORD@db/kanboard
    networks:
      my_bridge:
    ports:
      - 4480:80
      - 4443:443
    image: 'kanboard/kanboard:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# Kea
<details>
  <summary>
  </summary>

```
  kea:
    container_name: kea-dhcp4
    restart: $ALWAYS_ON_POLICY
    hostname: kea
    network_mode: host
    command: -c /kea/config/dhcp4.json
    volumes:
      - $PERSIST/kea/config:/kea/config
      - $PERSIST/kea/sockets:/kea/sockets
      - $PERSIST/kea/leases:/kea/leases
      - $PERSIST/kea/logs:/kea/logs
    labels: 
      monocker.enable: $MONOCKER_ENABLE
    image: 'jonasal/kea-dhcp4:2' # do not use the alpine as it does not support routing to Adguard
```
</details>

[🔼 Back to top](#networking-and-security)


# Lets Encrypt
<details>
  <summary>
  </summary>

```
  letsencrypt:
    container_name: letsencrypt
    restart: $ALWAYS_ON_POLICY
    hostname: letsencrypt
    volumes:
      - $PERSIST/homeassistant/ssl:/etc/letsencrypt:rw
    environment:
      - DUCKDNS_DOMAIN=$DOMAINNAME
      - DUCKDNS_TOKEN=$DUCKTOKEN
      - LETSENCRYPT_EMAIL=$GM_USER
      - LETSENCRYPT_DOMAIN=$DOMAINNAME
      - LETSENCRYPT_WILDCARD=ture
      - TZ=$TZ
    networks:
      my_bridge:
    image: 'maksimstojkovic/letsencrypt:latest'
```
</details>

[🔼 Back to top](#networking-and-security)


# LibreTranslate
<details>
  <summary>
  </summary>

```
  libretranslate:
    container_name: libretranslate
    restart: $RESTART_POLICY
    hostname: libretranslate
    environment:
      LT_LOAD_ONLY: en,ar #,de,zh,it,es,fr,pl,tr,ru,fi,nl,id,cs,el,hu,sk
      # LT_API_KEYS: true
      # LT_API_KEYS_DB_PATH: /app/db/api_keys.db # Same result as `db/api_keys.db` or `./db/api_keys.db`
      # LT_UPDATE_MODELS: true
    # volumes:
    #   - $PERSIST/libretranslate/db:/app/db
    #   - $PERSIST/libretranslate/models:/home/libretranslate/.local:rw
    healthcheck:
      test: ['CMD-SHELL', './venv/bin/python scripts/healthcheck.py']
    security_opt:
      - no-new-privileges:true
    ports:
      - 7821:5000
    networks: 
      my_bridge:
    image: 'libretranslate/libretranslate:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# LibreX
<details>
  <summary>
  </summary>

```
  librex:
    container_name: librex
    restart: $ALWAYS_ON_POLICY
    hostname: librex
    environment:
      - PUID=$PUID
      - PGID=$PGID
      - VERSION=docker
      - TZ=$TZ
      - CONFIG_GOOGLE_DOMAIN=com
      - CONFIG_GOOGLE_LANGUAGE_SITE=en
      - CONFIG_GOOGLE_LANGUAGE_RESULTS=en
      - CONFIG_WIKIPEDIA_LANGUAGE=en
    volumes:
      - $PERSIST/librex/nginx:/var/log/nginx
      - $PERSIST/librex/logs:/var/log/php7
    ports:
      - 8245:8080
    networks:
      my_bridge:
    image: 'librex/librex:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# Lingva-Translate
<details>
  <summary>
  </summary>

```
  lingva:
    container_name: lingva
    restart: $RESTART_POLICY
    hostname: lingva
    environment:
      site_domain: translate.$DOMAINNAME
      force_default_theme: dark
      default_source_lang: auto
      default_target_lang: en
    ports:
      - 6455:3000
    networks:
      my_bridge: 
    security_opt:
      - no-new-privileges:true
    image: 'thedaviddelta/lingva-translate:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# Memos
<details>
  <summary>
  </summary>

```
  memos:
    container_name: memos
    restart: $RESTART_POLICY
    hostname: memos
    volumes:
      - $PERSIST/memos:/var/opt/memos
    ports:
      - 5230:5230
    networks:
      my_bridge:
    labels: 
      # autoheal: $AUTOHEAL_RESTART
      monocker.enable: $MONOCKER_ENABLE
    image: 'neosmemo/memos:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# MiddleFinger
<details>
  <summary>
  </summary>

```
  middlefinger:
    container_name: middlefinger
    restart: $ALWAYS_ON_POLICY
    hostname: middlefinger
    ports:
      - 404:8080
    networks:
      my_bridge:
    image: 'modem7/middle-finger:dual'
```
</details>

[🔼 Back to top](#networking-and-security)


# Node-Exporter
<details>
  <summary>
  </summary>

```
  node-exporter:
    container_name: node-exporter
    restart: $RESTART_POLICY
    hostname: node-exporter
    privileged: true
    ports:
      - 9100:9100
    networks:
      my_bridge:
    image: 'prom/node-exporter:latest'
```
</details>

[🔼 Back to top](#docker-related)


# Node-RED
<details>
  <summary>
  </summary>

```
  nodered:
    container_name: nodered
    restart: $RESTART_POLICY
    hostname: nodered
    environment:
      - TZ=$TZ
    volumes:
      - $PERSIST/nodered:/data
    ports:
      - 1880:1880/tcp
    healthcheck:
      test: curl -fSs http://127.0.0.1:1880 || exit 1
      start_period: 90s
      timeout: 10s
      interval: 5s
      retries: 3    
    networks:
      my_bridge:
    working_dir: /usr/src/node-red
    labels:
      - autoheal=$AUTOHEAL_RESTART
    image: 'nodered/node-red:latest'
```
</details>

[🔼 Back to top](#databases)


# Nginx Proxy Manager
<details>
  <summary>
  </summary>

```
  npm:
    container_name: nginx-proxy-manager
    restart: $ALWAYS_ON_POLICY
    hostname: nginx-proxy-manager
    environment:
      PUID: $PUID
      PGID: $PGID
      TZ: $TZ
    #  DB_SQLITE_FILE: "/data/database.sqlite"
      DB_MYSQL_HOST: 'mariadb'
      DB_MYSQL_PORT: 3306
      DB_MYSQL_USER: 'npm'
      DB_MYSQL_PASSWORD: '$DB_PASSWORD'
      DB_MYSQL_NAME: 'npmdb'
      DISABLE_IPV6: 'true'
    volumes:
      - $PERSIST/nginx-proxy/letsencrypt:/etc/letsencrypt:rw
      - $PERSIST/nginx-proxy:/data:rw
    ports:
      - 880:80 
      - 881:81
      - 843:443
    networks:
      my_bridge:
    depends_on:
        mariadb:
          condition: service_healthy
    image: 'jc21/nginx-proxy-manager:latest'
```
</details>

[🔼 Back to top](#networking-and-security)


# Our Shopping List
<details>
  <summary>
  </summary>

```
  osl:
    container_name: osl
    restart: $ALWAYS_ON_POLICY
    hostname: osl
    environment:
      LISTEN_PORT: 8080
      MONGODB_HOST: osldb
      MONGODB_PORT: 27017
      MONGODB_DB: osl
      VUE_APP_I18N_LOCALE: en
      VUE_APP_I18N_FALLBACK_LOCALE: en
      VUE_APP_I18N_FORCE_LOCALE: 0
      VUE_APP_SINGLEBOARD_MODE: 0
    ports:
      - 8989:8080
    networks:
      my_bridge:
    depends_on:
      - osldb
    image: 'nanawel/our-shopping-list:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# Our Shopping List-DB
<details>
  <summary>
  </summary>

```
  osldb:
    container_name: osldb
    restart: $ALWAYS_ON_POLICY
    hostname: osldb
    volumes:
      - $PERSIST/mongodb/shoppinglist:/data/db
    ports:
      - 27017:27017
    networks:
      my_bridge:
    image: 'mongo:4.0'
```
</details>

[🔼 Back to top](#databases)


# Ouroboros
<details>
  <summary>
  </summary>

```
  ouroboros:
    container_name: ouroboros
    restart: $RESTART_POLICY
    hostname: ouroboros
    environment:
      - CLEANUP=true
      - LOG_LEVEL=info
      - CRON="0/15 1 * * *" #At every 15th minute from 0 through 59 past 1am
      - LATEST=true
      - SELF_UPDATE=true
      - SKIP_STARTUP_NOTIFICATIONS=true 
      - NOTIFIERS="pover://$PUSHOVER_USER_KEY@$PUSHOVER_OUROBOROS_API"
      #- IGNORE=esphome homeassistant
      - TZ=$TZ
    volumes:
      - $DOCKER_SOCKET:/var/run/docker.sock:rw
    networks:
      my_bridge:
    working_dir: /app
    image: 'pyouroboros/ouroboros:latest'
```
</details>

[🔼 Back to top](#docker-related)


# phpMyAdmin
<details>
  <summary>
  </summary>

```
  phpmyadmin:
    container_name: phpmyadmin
    restart: $RESTART_POLICY
    hostname: phpmyadmin
    environment:
      - PMA_HOST=mariadb
      - PMA_USER=root
      - PMA_PASSWORD=$DB_PASSWORD
      - TZ=$TZ
    volumes:
       - $LOCAL_TIME:/etc/localtime:ro
    ports:
      - 3300:80 
    networks:
      my_bridge:
    depends_on:
      - mariadb
    tty: true
    image: 'phpmyadmin/phpmyadmin:latest'
```
</details>

[🔼 Back to top](#databases)


# Phlare
<details>
  <summary>
  </summary>

```
  phlare:
    container_name: phlare
    restart: $RESTART_POLICY
    hostname: phlare
    volumes:
      - $PERSIST/phlare/config.yaml:/etc/phlare/config.yaml'
    ports:
      - 4100:4100
    networks:
      my_bridge:
    command: --config.file=/etc/phlare/config.yaml
    image: 'grafana/phlare:latest'
```
</details>

[🔼 Back to top](#databases)


# Photoprism
<details>
  <summary>
  </summary>

```
  photoprism:
    container_name: photoprism
    restart: $RESTART_POLICY
    hostname: photoprism
    security_opt:
      - seccomp:unconfined
      - apparmor:unconfined
    user: $PUID:$PGID
    healthcheck:
      test: wget --no-verbose --tries=1 --spider http://localhost:2342
    ports:
      - 2342:2342        
    volumes:
      - $PERSIST/photoprism/import:/photoprism/import:rw       # *Optional* base folder from which files can be imported to originals    
      - $PERSIST/photoprism/storage:/photoprism/storage:rw
      - $MEDIA_PATH/Photos:/photoprism/originals:rw
#     - /volume1/docker/photoprism/family:/photoprism/originals/family:rw               # *Additional* media folders can be mounted like this
    environment:
      PHOTOPRISM_ADMIN_USER: tam
      PHOTOPRISM_ADMIN_PASSWORD: ts*JhcmL&5zc^5
      PHOTOPRISM_UID: $PUID
      PHOTOPRISM_GID: $PGID
      PHOTOPRISM_AUTH_MODE: password
      PHOTOPRISM_SITE_URL: http://localhost:2342/
      PHOTOPRISM_ORIGINALS_LIMIT: 5120
      PHOTOPRISM_HTTP_COMPRESSION: gzip
      PHOTOPRISM_READONLY: false
      PHOTOPRISM_EXPERIMENTAL: false
      PHOTOPRISM_DISABLE_CHOWN: false
      PHOTOPRISM_DISABLE_WEBDAV: false
      PHOTOPRISM_DISABLE_SETTINGS: false
      PHOTOPRISM_DISABLE_TENSORFLOW: false
      PHOTOPRISM_DISABLE_FACES: false
      PHOTOPRISM_DISABLE_CLASSIFICATION: false
      PHOTOPRISM_DISABLE_RAW: true
      PHOTOPRISM_RAW_PRESETS: false
      PHOTOPRISM_DISABLE_DARKTABLE: true
      PHOTOPRISM_DISABLE_RAWTHERAPEE: true
      PHOTOPRISM_DISABLE_IMAGEMAGICK: true
      PHOTOPRISM_DISABLE_HEIFCONVERT: true
      PHOTOPRISM_DISABLE_RSVGCONVERT: true
      PHOTOPRISM_JPEG_QUALITY: 100
      PHOTOPRISM_DETECT_NSFW: false
      PHOTOPRISM_UPLOAD_NSFW: true
      PHOTOPRISM_SPONSOR: true
      PHOTOPRISM_DATABASE_DRIVER: mysql
      PHOTOPRISM_DATABASE_SERVER: mariadb:3306
      PHOTOPRISM_DATABASE_NAME: photoprism
      PHOTOPRISM_DATABASE_USER: photoprism
      PHOTOPRISM_DATABASE_PASSWORD: $DB_PASSWORD
      PHOTOPRISM_WORKERS: 2
      PHOTOPRISM_THUMB_FILTER: blackman       # best to worst: blackman, lanczos, cubic, linear
      PHOTOPRISM_APP_MODE: standalone         # progressive web app MODE - fullscreen, standalone, minimal-ui, browser
#     PHOTOPRISM_SITE_CAPTION: "AI-Powered Photos App"
#     PHOTOPRISM_SITE_DESCRIPTION: ""
#     PHOTOPRISM_SITE_AUTHOR: ""
    working_dir: "/photoprism"
    networks:
      my_bridge:
    depends_on:
      mariadb:
        condition: service_started
    image: 'photoprism/photoprism:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# Photoview
<details>
  <summary>
  </summary>

```
  photoview:
    container_name: photoview
    restart: $RESTART_POLICY
    hostname: photoview
    environment:
      PHOTOVIEW_DATABASE_DRIVER: postgres
      PHOTOVIEW_POSTGRES_URL: postgresql://photoview:photoview@photoview-postgress:5432/photoview
      PHOTOVIEW_LISTEN_PORT: 4000
      PHOTOVIEW_MEDIA_CACHE: /app/cache
    volumes:
      - $PERSIST/photoview/cache:/app/cache:rw
      - $MEDIA_PATH/Photos:/photos:ro
      # - /path_to_photos:/photos2:ro
    ports:
      - 7354:4000
    networks:
      my_bridge:
    security_opt:
      - no-new-privileges:true
    read_only: true
    user: $PUID:$PGID
    healthcheck:
      test: curl -f http://localhost:4000/ || exit 1
    depends_on:
      photoview-postgress:
        condition: service_healthy
    image: 'viktorstrate/photoview:2'
```
</details>

[🔼 Back to top](#self-hosted)


# Photoview-Postgress
<details>
  <summary>
  </summary>

```
  photoview-postgress:
    container_name: photoview-postgress
    restart: $RESTART_POLICY
    hostname: photoview-postgress
    environment:
      POSTGRES_DB: photoview
      POSTGRES_USER: photoview
      POSTGRES_PASSWORD: photoview
    volumes:
      - $PERSIST/photoview/db:/var/lib/postgresql/data:rw
    networks:
      my_bridge:  
    security_opt:
      - no-new-privileges:true
    user: $PUID:$PGID
    healthcheck:
      test: ["CMD", "pg_isready", "-q", "-d", "photoview", "-U", "photoview"]
      timeout: 45s
      interval: 10s
      retries: 10
    image: 'postgres:alpine'
```
</details>

[🔼 Back to top](#databases)


# PicoShare
<details>
  <summary>
  </summary>

```
  picoshare:
    container_name: picoshare
    restart: $RESTART_POLICY
    hostname: picoshare
    environment:
      - PORT=3001
      - PS_SHARED_SECRET=$PASSWD # Change to any password
    volumes:
      - $PERSIST/picoshare:/data
    ports:
      - 8179:3001
    networks:
      my_bridge:
    command: -db /data/store.db -vacuum true
    image: 'mtlynch/picoshare:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# Pigallery
<details>
  <summary>
  </summary>

```
  pigallery:
    container_name: pigallery
    hostname: pigallery
    restart: $RESTART_POLICY
    environment:
      NODE_ENV: production
      PORT: 8080
    volumes:
      - $PERSIST/pigallery/db:/app/data/db:rw
      - $PERSIST/pigallery/config:/app/data/config:rw
      - $PERSIST/pigallery/tmp:/app/data/tmp:rw
      - $MEDIA_PATH/Photos:/app/data/images:ro  # Point to your photos folder
    ports:
      - 9842:8080
    networks:
      my_bridge:  
    security_opt:
      - no-new-privileges:true
    read_only: true
    user: $PUID:$PGID
    healthcheck:
      test: wget --no-verbose --tries=1 --spider http://localhost:8080
    image: 'bpatrik/pigallery2:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# Piped-Postgres
<details>
  <summary>
  </summary>

```
  piped-postgres:
    container_name: piped-postgres
    hostname: piped-postgres
    restart: $RESTART_POLICY
    volumes:
      - $PERSIST/piped/db:/var/lib/postgresql/data
    environment:
      TZ: $TZ
      POSTGRES_DB: piped
      POSTGRES_USER: piped
      POSTGRES_PASSWORD: $DB_PASSWORD
    networks:
      my_bridge:
    security_opt:
      - no-new-privileges:true
    healthcheck:
      test: ["CMD", "pg_isready", "-q", "-d", "piped", "-U", "piped"]
      timeout: 45s
      interval: 10s
      retries: 10
    labels: 
      autoheal: $AUTOHEAL_RESTART
    user: $PUID:$PGID
    image: 'postgres:alpine'
```
</details>

[🔼 Back to top](#databases)


# Piped-Backend
<details>
  <summary>
  </summary>

```
  piped-backend:
    container_name: piped-backend
    hostname: piped-backend
    restart: $RESTART_POLICY
    volumes:
      - $PERSIST/piped/config.properties:/app/config.properties:ro
    networks:
      my_bridge:
    healthcheck:
      test: stat /etc/passwd || exit 1
    labels: 
      autoheal: $AUTOHEAL_RESTART
      monocker.enable: $MONOCKER_ENABLE
    security_opt:
      - no-new-privileges:true
    depends_on:
      piped-postgres:
        condition: service_healthy
    image: '1337kavin/piped:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# Piped-Frontend
<details>
  <summary>
  </summary>

```
  piped-frontend:
    container_name: piped-frontend
    hostname: piped-frontend
    restart: $RESTART_POLICY
    networks:
      my_bridge:
    security_opt:
      - no-new-privileges:true
    depends_on:
      piped-backend:
        condition: service_healthy
    entrypoint: ash -c 'sed -i s/pipedapi.kavin.rocks/utubeapi.tamimology.com/g /usr/share/nginx/html/assets/* && /docker-entrypoint.sh && nginx -g "daemon off;"'
    healthcheck:
      test: wget --no-verbose --tries=1 --spider http://localhost:80
    labels: 
      autoheal: $AUTOHEAL_RESTART
      monocker.enable: $MONOCKER_ENABLE
    image: '1337kavin/piped-frontend:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# Piped-Nginx
<details>
  <summary>
  </summary>

```
  piped-nginx:
    container_name: piped-nginx
    restart: $RESTART_POLICY
    hostname: piped-nginx
    ports:
      - 7601:80
    networks:
      my_bridge:
    volumes:
      - $PERSIST/piped/proxy:/var/run/ytproxy:rw
      - $PERSIST/piped/nginx/nginx.conf:/etc/nginx/nginx.conf:ro
      - $PERSIST/piped/nginx/pipedapi.conf:/etc/nginx/conf.d/pipedapi.conf:ro
      - $PERSIST/piped/nginx/pipedproxy.conf:/etc/nginx/conf.d/pipedproxy.conf:ro
      - $PERSIST/piped/nginx/pipedfrontend.conf:/etc/nginx/conf.d/pipedfrontend.conf:ro
      - $PERSIST/piped/nginx/ytproxy.conf:/etc/nginx/snippets/ytproxy.conf:ro
    security_opt:
      - no-new-privileges:true
    healthcheck:
      test: wget --no-verbose --tries=1 --spider http://localhost:80
    labels: 
      autoheal: $AUTOHEAL_RESTART
      monocker.enable: $MONOCKER_ENABLE
    depends_on:
      piped-backend:
        condition: service_healthy
      piped-frontend:
        condition: service_healthy
      piped-proxy:
        condition: service_started
    image: 'nginx:mainline-alpine'
```
</details>

[🔼 Back to top](#self-hosted)


# Piped-Proxy
<details>
  <summary>
  </summary>

```
  piped-proxy:
    container_name: piped-proxy
    restart: $RESTART_POLICY
    hostname: piped-proxy
    environment:
      UDS: 1
    volumes:
      - $PERSIST/piped/proxy:/app/socket:rw
    networks:
      my_bridge:
    read_only: true
    security_opt:
      - no-new-privileges:true
    image: '1337kavin/piped-proxy:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# Piwigo
<details>
  <summary>
  </summary>

```
  piwigo:
    container_name: piwigo
    restart: $RESTART_POLICY
    hostname: piwigo
    volumes:
      - $PERSIST/piwigo/config:/config
      - $MEDIA_PATH/Photo Albums:/gallery
    environment:
      - PUID=$PUID
      - PGID=$PGID
      - TZ=$TZ
    healthcheck:
      test: curl -fSs http://127.0.0.1:80 || exit 1
      start_period: 120s
      timeout: 10s
      interval: 5s
      retries: 3
    ports:
      - 8100:80
    networks:
      my_bridge:
    labels:
      - autoheal=$AUTOHEAL_RESTART
    depends_on:
      - piwigodb
    image: 'ghcr.io/linuxserver/piwigo:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# Piwigo-DB
<details>
  <summary>
  </summary>

```
  piwigodb:
    container_name: piwigo-db
    restart: $RESTART_POLICY
    hostname: piwigodb
    volumes:
      - $PERSIST/piwigo/db:/config
    environment:
      - PUID=$PUID
      - PGID=$PGID
      - TZ=$TZ
      - MYSQL_ROOT_PASSWORD=piwigo
      - MYSQL_DATABASE=piwigo
      - MYSQL_USER=piwigo
      - MYSQL_PASSWORD=$DB_PASSWORD
    healthcheck:
      test: ["CMD", "mysqladmin", "ping", "--silent"]
    expose: 
      - 3306
    networks:
      my_bridge:
    image: 'jbergstroem/mariadb-alpine:10.6.13'
```
</details>

[🔼 Back to top](#databases)


# Prometheus
<details>
  <summary>
  </summary>

```
  prometheus:
    container_name: prometheus
    restart: $RESTART_POLICY
    hostname: prometheus
    volumes:
      - $PERSIST/prometheus/data:/prometheus
      - $PERSIST/prometheus/prometheus.yml:/etc/prometheus/prometheus.yml
    depends_on:
        - cadvisor
    ports:
      - 9090:9090
    networks:
      my_bridge:
    user: root
    command: "--config.file=/etc/prometheus/prometheus.yml --storage.tsdb.path=/prometheus"
    image: 'prom/prometheus:latest' 
```
</details>

[🔼 Back to top](#databases)


# Plex Media Server
<details>
  <summary>
  </summary>

```
  plex:
    container_name: plex
    restart: $RESTART_POLICY
    hostname: plex
    environment:
      - PLEX_CLAIM=$PLEX_CLAIM_TOKEN
      - PLEX_GID=$PGID
      - PLEX_UID=$PUID
      - TZ=$TZ
    volumes:
      - $PERSIST/plex/config:/config:rw
      - $PERSIST/plex/transcode:/transcode:rw
      - $PERSIST/plex/data:/data:rw
      - $MEDIA_PATH:/media:rw
    healthcheck:
      test: curl -fsS http://localhost:32400/identity > /dev/null || exit 1
      start_period: 60s
      timeout: 10s
      interval: 5s
      retries: 3
    #network_mode: "host"
    networks:
      my_bridge:
    labels:
      - autoheal=$AUTOHEAL_RESTART
    image: 'plexinc/pms-docker:latest'
```
</details>

[🔼 Back to top](#media-playing)


# Portainer-CE
<details>
  <summary>
  </summary>

```
  portainer:
    container_name: portainer-ce
    restart: $ALWAYS_ON_POLICY
    hostname: portainer
    privileged: true
    command: -H $DOCKER_HOST --tlsskipverify
    environment:
      DOCKER_HOST: $SOCKET
    volumes:
      - $DOCKER_SOCKET:/var/run/docker.sock
      - $PERSIST/portainer:/data
    ports:
      - 8000:8000/tcp
      - 9000:9000/tcp
      - 9443:9443/tcp
    networks:
      my_bridge:
    image: 'portainer/portainer-ce:alpine'
```
</details>

[🔼 Back to top](#docker-related)


# Reiverr
<details>
  <summary>
  </summary>

```
  reiverr:
    container_name: reiverr
    restart: $RESTART_POLICY
    hostname: reiverr
    volumes:
      - $PERSIST/reiverr:/config
    ports:
      - 9494:9494
    networks:
      my_bridge:
    image: 'ghcr.io/aleksilassila/reiverr:latest'
```
</details>

[🔼 Back to top](#media-playing)


# SearXNG
<details>
  <summary>
  </summary>

```
  searxng:
    container_name: searxng
    restart: $ALWAYS_ON_POLICY
    hostname: searxng
    environment:
      - SEARXNG_BASE_URL=https://search.$DOMAINNAME/
    volumes:
      - $PERSIST/searxng:/etc/searxng:rw
    ports:
     - 5080:8080
    networks:
      my_bridge:
    cap_drop:
      - ALL
    cap_add:
      - CHOWN
      - SETGID
      - SETUID
      - DAC_OVERRIDE
    logging:
      driver: "json-file"
      options:
        max-size: "1m"
        max-file: "1"
    image: 'searxng/searxng:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# Shiori
<details>
  <summary>
  </summary>

```
  shiori:
    container_name: shiori
    restart: $RESTART_POLICY
    hostname: shiori
    environment:
      - PUID=1000
      - PGID=1000
    ports:
        - 18080:8080
    networks:
      my_bridge:
    volumes:
        - $PERSIST/shiori:/shiori
    image: 'ghcr.io/go-shiori/shiori:latest'
```
</details>

[🔼 Back to top](#links-and-page-organisation)


# SnipeIT
<details>
  <summary>
  </summary>

```
  snipeit:
    image: 'snipe/snipe-it:latest-alpine' #https://hub.docker.com/r/snipe/snipe-it/
    container_name: snipeit
    restart: on-failure:5
    healthcheck:
      test: curl -f http://localhost:80/ || exit 1
    depends_on:
      - mariadb
    networks:
      my_bridge:
    volumes:
      - $PERSIST/snipeit:/config:rw
    environment:
      - TZ=$TZ
      - APP_URL=http://192.168.1.10:1339
      - APP_KEY=base64:jtww95XJZi2u97Nou8a9SMI2CZg6BOvfdUc7yF9y4bw=
      - MYSQL_PORT_3306_TCP_ADDR=mariadb
      - MYSQL_PORT_3306_TCP_PORT=3306
      - MYSQL_DATABASE=snipe
      - MYSQL_USER=snipe
      - MYSQL_PASSWORD=snipe
      - PGID=$PGID
      - PUID=$PUID
      - MAIL_PORT_587_TCP_ADDR=$GM_HOST
      - MAIL_PORT_587_TCP_PORT=$GM_PORT
      - MAIL_ENV_FROM_ADDR=$GM_USER
      - MAIL_ENV_FROM_NAME=Tamimology Server
      - MAIL_ENV_ENCRYPTION=SSL
      - MAIL_ENV_USERNAME=$GM_USER
      - MAIL_ENV_PASSWORD=$GM_PSW
    ports:
      - 1339:80
```
</details>

[🔼 Back to top](#networking-and-security)


# TLS-Socket-Proxy
<details>
  <summary>
  </summary>
  
```
  dsp:
    container_name: dsp
    restart: $ALWAYS_ON_POLICY
    hostname: dsp
    environment:
      CERTS_DIR: "/server-keys"
    volumes:
      - $PERSIST/socket-proxy/certs:/server-keys:ro #https://docs.docker.com/engine/security/protect-access/
      - $DOCKER_SOCKET:/var/run/docker.sock
    ports:
      - 2376:2376
    command: ["-dd", "-lmlocal2"]
    image: 'sjawhar/docker-socket-proxy:latest'
```
</details>

[🔼 Back to top](#docker-related)


# Vault
<details>
  <summary>
  </summary>
  
```
  vault:
    restart: on-failure:5
    container_name: vault
    environment:
      VAULT_DEV_LISTEN_ADDRESS: 0.0.0.0:8200
    volumes:
      - $PERSIST/vault/logs:/vault/logs:rw
      - $PERSIST/vault/data:/vault/file:rw
      - $PERSIST/vault/config:/vault/config:rw
      - $PERSIST/vault/plugins:/vault/plugins:rw
      - $LOCAL_TIME:/etc/localtime:ro
    # hostname: vault
    # mem_limit: 512m
    # cpu_shares: 768
    ports:
      - 8205:8200
    networks:
      my_bridge:
    security_opt:
      - no-new-privileges:true
    cap_add:
      - IPC_LOCK
    entrypoint: vault server -config=/vault/config/vault.json
    healthcheck:
      test: wget --no-verbose --tries=1 --spider http://localhost:8200
    image: 'hashicorp/vault:latest'
```
</details>

[🔼 Back to top](#self-hosted)


# Visual Studio Code
<details>
  <summary>
  </summary>

```
  vscode:
    container_name: vscode
    restart: $RESTART_POLICY
    hostname: vscode
    environment:
      - PASSWORD=$DB_PASSWORD
    volumes:
      - $PERSIST/vscode:/home/coder/.local/share/code-server:rw
      - $PERSIST/homeassistant:/home/coder/project:rw
    ports:
      - 8181:8080
    networks:
      my_bridge:
    working_dir: /home/coder
    command: code-server --no-auth # if using proxy add: --allow-http
    image: 'codercom/code-server:latest'
```
</details>

[🔼 Back to top](#programming)


# Container-WebTTY
<details>
  <summary>
  </summary>

```
  webtty:
    container_name: container-webtty
    restart: $RESTART_POLICY
    hostname: webtty
    environment:
      - DOCKER_HOST=$SOCKET
    # volumes:
    #   - $DOCKER_SOCKET:/var/run/docker.sock
    ports:
      - 8818:8080
    networks:
      my_bridge:
    image: 'wrfly/container-web-tty:0.1.10' #latest'
```
</details>

[🔼 Back to top](#docker-related)


# WhatsUpDocker
<details>
  <summary>
  </summary>

```
  whatsupdocker:
    container_name: whatsupdocker
    restart: $RESTART_POLICY
    hostname: whatsupdocker
    environment:
      - DOCKER_HOST=$SOCKET  
      # - TZ=$TZ
      - WUD_TRIGGER_PUSHOVER_1_TOKEN=$PUSHOVER_DOCKER_UPDATES_API
      - WUD_TRIGGER_PUSHOVER_1_USER=$PUSHOVER_USER_KEY
      # - WUD_TRIGGER_PUSHOVER_1_DEVICE=myIphone,mySamsung
      - WUD_WATCHER_LOCAL_CRON=0 10 * * *
    volumes:
    #   - /var/run/docker.sock:/var/run/docker.sock
      - $PERSIST/wud:/store
      - $LOCAL_TIME:/etc/localtime:ro
    healthcheck:
      test: wget --no-verbose --tries=1 --no-check-certificate --spider http://localhost:3000
      interval: 10s
      timeout: 10s
      retries: 3
      start_period: 10s  
    networks:
      my_bridge:
    ports:
      - 3000:3000
    image: 'fmartinou/whats-up-docker:latest'
```
</details>

[🔼 Back to top](#docker-related)


# WebDAV
<details>
  <summary>
  </summary>

```
  webdav:
    container_name: webdav
    restart: $RESTART_POLICY
    hostname: webdav
    environment:
      PUID: $PUID
      PGID: $PGID
      AUTH_TYPE: Digest
      USERNAME: admin
      PASSWORD: $WEBDAVPWD
    volumes:
      - $PERSIST/webdav:/var/lib/dav
    ports:
      - 8888:80
    networks:
      my_bridge:
    image: 'bytemark/webdav:latest'
```
</details>

[🔼 Back to top](#networking-and-security)


# webNUT
<details>
  <summary>
  </summary>

```
  webnut:
    container_name: webnut
    restart: $ALWAYS_ON_POLICY
    hostname: webnut
    environment:
      UPS_HOST: "$LOCAL_HOST"
      UPS_PORT: "3493"
      UPS_USER: "upsadmin"
      UPS_PASSWORD: "secret"
    ports:
     - 6543:6543
    network_mode: host
    image: 'edgd1er/webnut:latest'
```
</details>

[🔼 Back to top](#networking-and-security)


# WeTTY
<details>
  <summary>
  </summary>

```
  wetty:
    container_name: wetty
    restart: $RESTART_POLICY
    hostname: wetty
    environment:
      SSHHOST: $LOCAL_HOST
      SSHPORT: 22
      SSHUSER: admin
      SSHPASS: $SSHPASS
      NODE_ENV: 'development'
      BASE: /
      VERSION: true
      # COMMAND: 'cd $PERSIST'
      TITLE: 'My SSH'
    ports:
      - 5800:3000
    networks:
      my_bridge:
        ipv4_address: $BRIDGE_NET.100
    tty: true
    working_dir: /usr/src/app
    image: 'wettyoss/wetty:latest'
```
</details>

[🔼 Back to top](#docker-related)


# xBackbone
<details>
  <summary>
  </summary>

```
  xbackbone:
    container_name: xbackbone
    restart: $RESTART_POLICY
    hostname: xbackbone
    environment:
      - PUID=$PUID
      - PGID=$PGID
      - TZ=$TZ
    volumes:
      - $PERSIST/xbackbone:/config
    ports:
      - 8522:80
      - 4443:443
    networks:
      my_bridge: 
    image: 'lscr.io/linuxserver/xbackbone:latest'
```
</details>

[🔼 Back to top](#self-hosted)
