# Mattermost World Cup Bot
**NOTE:** This is a fork of [ImDevinC/wc-watcher](https://github.com/ImDevinC/wc-watcher) which is updated for mattermost and dockerized(pending)

This bot uses the undocumented FIFA API's to report on World Cup matches. It will check every 60 seconds for new events. The following events are reported:
+ Goals scored
+ Yellow/Red cards
+ Substitutions
+ Match start/stop
+ Penalty kicks missed/scored

### Sample
[![sample](https://github.com/ImDevinC/wc-watcher/raw/master/ss.png)](#sample)

### Quick start using docker
A simple `Dockerfile` is including for deploying this in a docker container.
1. Clone the repo and navigate into the directory
1. Make necessary changes to private.py (see instructions below, at the minimum your webhook url has to be updated)
1. Build the docker image locally by running the following command, you can change the image name to your liking
```
docker build -t thiyag/wc-watcher .
```
1. Once the image is built, run the process in a new container, this runs as a daemon (`-d`)
```
docker run -d --name wcbot thiyag/wc-watcher
```
1. To update the code and or config, rebuild the image, and before issuing `docker run`, delete/remove the existing container
```
docker stop wcbot
docker rm wcbot
```

### Usage
1. Setup a new Mattermost incoming webhook and retain the url to use in `WEBHOOK_URL` below
1. Copy `private.py.config` to `private.py`
1. In `private.py`, change `WEBHOOK_URL` to point to your Slack webhook
    + If you want to see debug information, which currently pings a heartbeat every hour, also fill in the `DEBUG_WEBHOOK` url with a Mattermost webhook and set `DEBUG = True`
    + You can also set `WC_COMPETITION = None` in `soccerbot.py` to get all current FIFA matches and see what the output looks like. Just make sure to change it back to `WC_COMPETITION = 17` for world cup only
1. In `private.py`, leave the `CHANNEL` blank, mattermost's incoming webhook will already have the channel it would post to, entering a channel name or id would cause the webhook to fail
1. Use `pip install -r requirements.txt`
1. Run `python soccerbot.py`

### Card emoji
1. Go to the `Custom Emoji` screen in mattermost
1. Enter `yellow_card_new` as name
1. Upload `hand_yellow_card.png`
1. Save emoji

Repeat for `red_card_new` and `hand_red_card.png`
