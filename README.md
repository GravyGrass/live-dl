[![Docker Publish](https://github.com/herowinb/live-dl/actions/workflows/docker-publish.yml/badge.svg)](https://github.com/herowinb/live-dl/actions/workflows/docker-publish.yml) [![Docker Pulls](https://img.shields.io/docker/pulls/herowinb/live-dl.svg)](https://hub.docker.com/r/herowinb/live-dl)

# [Bug report](https://github.com/herowinb/live-dl/issues/new/choose)

# `New feature`
 * [Run on Docker](#how-run-on-synology-nas-docker)
 * Site support:
   * Youtube.com (and youtu.be)
   * Twitch.tv
 * Metadata write
 * Archive live chat. Supported member-only stream. 2 modes:
   * Simple: Txt file, easy to read. You can change format in `/config/chat-format.json`
   * Full: Json file, playable via  https://archive.ragtag.moe/player
 * Keyword filter (Not apply to Twitch)
 * [Member only (channel and video) support ](#how-monitor-member-only-stream) (Not apply to Twitch)
 * [Add option Long interval loop](#how-use-long-interval) (Not apply to Twitch)
 * Notify (CLI, Discord and Email) when new live-dl updates are available
 * [Discord notification](#how-use-Discord-notification)
 * Audio only mode (Not apply to Twitch)
 * No log mode
 * Use [YTARCHIVE.py](https://github.com/Kethsar/ytarchive) to download Youtube Live Stream. If you not change config, Live-dl still use default Youtube-dl

<img src="https://i.imgur.com/kIJUOIs.png">

# [`live-dl`](https://github.com/sparanoid/live-dl)

Download files location (default): `/youtube-dl/VTuber Recordings`

# [`youtube-dl-server`](https://github.com/manbearwiz/youtube-dl-server)

Download files location (default): `/youtube-dl/`

Web GUI : `http://{{host}}:8080/`

# How run on Synology NAS Docker

Install `Docker` Package in Synology `Package Center`, run Docker and search my Docker Image `herowinb/live-dl` in Registry tab with keyword `live-dl`, after download launch Image and edit your config.

 * Execute container using high privilege
 * Enable Auto-restart
 * If you want Live-dl running with your Timezone, please add an Environment, Variables: `TZ` and Value: `Etc/GMT+7` or check [here](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones)
 * Port setting, change Local port from `Auto` to `8080`
 * Add Volume follow this:
<img src="https://i.imgur.com/kQv3g0W.png">

Download [Zip](https://github.com/herowinb/live-dl/archive/master.zip) and unzip `Config` folder, rename `config.example.yml` to `config.yml` and edit.

Edit `auto.sh`, remove # and change channel URL. Now you can startup `live-dl` to monitor and download upcoming live stream from your ~~Waifu~~ Vtuber.

Edit `keyword.txt`, only download if match with keyword filter, using `\|` between 2 keywords

After finish your config, run your new Container.

Use live-dl

<img src="https://i.imgur.com/5uFLJtr.png">

You can run this script in background (manually):

```shell
# Start process
nohup bash live-dl https://www.youtube.com/channel/UC1opHUrw8rvnsadT-iGp7Cg &>/tmp/live-dl-minatoaqua.log &

# View processes
ps aux | grep live-dl
501 94552   964   0  9:38PM ttys009    0:00.06 bash live-dl https://www.youtube.com/channel/UC1opHUrw8rvnsadT-iGp7Cg
501 94765   964   0  9:39PM ttys009    0:00.00 grep live-dl

# Stop process
kill 94552
```

## How update new image docker (new live-dl) on Synology NAS Docker

Search my Docker Image `herowinb/live-dl` in Registry tab with keyword `live-dl`. Wait to complete download new image.
Stop your Container `live-dl` running, update/edit new `config.yml`, `keyword.txt` or new config file … (if need). On your Container `live-dl` select Action and Clear then Start your Container again, it will update to new Image `live-dl`.

## How monitor member only stream

This feature help who joined membership but cannot watch live (time zone problem, busy with work or life) but don't want to miss out un-archive member only streams. Please remember that SHARING with non-members is **PROHIBITED**.

You need to login YouTube with an account joined membership of the channel to monitor, then export `cookies.txt` (for Youtube.com only) using the [Firefox addon](https://addons.mozilla.org/en-US/firefox/addon/cookies-txt/) or [Chrome Extension](https://chrome.google.com/webstore/detail/get-cookiestxt/bgaddhkoddajcdgocldbbfleckgcbcid?hl=en). Rename to `cookies_membership.txt` and move it to `config` folder.

Note:
  * You should **NOT** replace your `cookies.txt` with `cookies.txt` generated by `live-dl`. You can use `LONG INTERVAL` below to avoid YouTube seeing your account as a bot.
  * You can not take archive chat with member only stream.
  * Cookies may expire after a while of use and you need to update/replace it. A warning message will be sent via discord if cookies are expired.
  * In case Live-dl can not detect member-only Video ID, you can use option `-mb` to manual monitor/download.

  ```shell
  live-dl -mb https://www.youtube.com/watch?v=xxxxxxx
  ```

## How use long interval

New option `LONG INTERVAL` in `config.yml`, it helps live-dl not run too much. If detect upcoming time longer than `LONG INTERVAL`, live-dl will use `LONG INTERVAL`.

You should set `LONG INTERVAL` between 600 (10 minutes) and 1800 (30 minutes) to make sure `live-dl` not miss guerrilla/RTA stream.

```shell
live-dl -i 60 -li 900 https://www.youtube.com/channel/UCxxxxxxxxxxxx/community
```

## How use Discord notification

Push notification via Discord DM. If you want to push notification for a channel, please use Pingcord.

Create an Application and a Bot at https://discord.com/developers/, copy Bot token to `config.yml` file. Because Bot can not create a DM to User at first time and User can not add friend or find Bot to make a DM, you need follow this:
* Create a temp Discord server, add your Bot to temp server (change xxxx = client ID in application) https://discord.com/api/oauth2/authorize?client_id=xxxx&scope=applications.commands
* Click to Bot on Server User list (right panel) and send a Private Message to bot
* Open https://discord.com/channels/@me/ on browser, select your Bot and copy channel ID (numbers after /@me/, put **between two `"`**) in to `config.yml` file
* You can delete temp Discord server

<img src="https://i.imgur.com/jJIRwlH.png">

# Docker Desktop for Windows (Docker Desktop Community 2.4.0.0)

Get [Docker install pack](https://www.docker.com/products/docker-desktop) and [WSL2 update](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi).
After install WSL2 and Docker, open CMD and type `docker pull herowinb/live-dl:latest` to get latest version from Docker Hub.

Create 1 folder for download files. Download and unzip [Zip](https://github.com/herowinb/live-dl/archive/master.zip), rename (or edit your config) `config.example.yml` to `config.yml` and edit `example.sh` and `keyword.txt` in `config` folder to monitor and download live stream.

Open Docker Dashboard, check downloaded images in LOCAL, click Run and add some Optional Settings

<img src="https://i.imgur.com/lj0WQw7.png">

<img src="https://i.imgur.com/QiXxFQl.png">

CLI
<img src="https://i.imgur.com/uVssi9f.png">
