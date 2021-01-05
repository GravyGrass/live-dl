# [`New feature`]
 * Run on Docker
 * Metadata write
 * Archive live chat (Note: using this feature may make a large size json file if live chat has a high number of users/spam chat or spam emo.)
 * Keyword filter

<img src="https://i.imgur.com/tz3RpU8.png">

# [`live-dl`](https://github.com/sparanoid/live-dl)

Download files location (default): `/youtube-dl/VTuber Recordings`

# [`youtube-dl-server`](https://github.com/manbearwiz/youtube-dl-server)

Download files location (default): `/youtube-dl/`

Web gui : `http://{{host}}:8080/youtube-dl`

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

You can run this script in background (manualy):

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

## How update new image (new live-dl) on Synology NAS Docker

Search my Docker Image `herowinb/live-dl` in Registry tab with keyword `live-dl`. Wait to complete download new image.
Stop your Container `live-dl` running, update/edit new `config.yml`, `keyword.txt` or new config file ... (if need). On your Container `live-dl` select Action and Clear then Start your Container again, it will update to new Image `live-dl`.

# Docker Desktop for Windows (Docker Desktop Community 2.4.0.0)

Get [Docker install pack](https://www.docker.com/products/docker-desktop) and [WSL2 update](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi).
After install WSL2 and Docker, open CMD and type `docker pull herowinb/live-dl:latest` to get latest version from Docker Hub.

Create 1 folder for download files. Download and unzip [Zip](https://github.com/herowinb/live-dl/archive/master.zip), rename (or edit your config) `config.example.yml` to `config.yml` and edit `example.sh` and `keyword.txt` in `config` folder to monitor and download live stream.

Open Docker Dashboard, check downloaded images in LOCAL, click Run and add some Optional Settings

<img src="https://i.imgur.com/lj0WQw7.png">

<img src="https://i.imgur.com/QiXxFQl.png">

CLI
<img src="https://i.imgur.com/uVssi9f.png">
