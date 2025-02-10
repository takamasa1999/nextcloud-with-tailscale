# Nextcloud All-in-One with Tailscale 
This repository is made to save the settings that I tried to run [Nextcloud All-in-One](https://github.com/nextcloud/all-in-one) with domain names provided by [Tailscale](https://github.com/tailscale/tailscale) on my home server.  
This project is primarily made for my self to save the best settings on my server environment. If this accidentally helps you somehow, that's more than a pleasure.


## local
Runs only within your Tailscale network.
[Found the seed of this solution here](https://github.com/nextcloud/all-in-one/discussions/5439#discussioncomment-11935630)

## public
Runs on internet so that you can share with anybody without inviting them to your Tailscale network.
[Found the seed of this solution here](https://github.com/nextcloud/all-in-one/discussions/5439#discussioncomment-11696448)

## public-hostscale (preferred)
This is intended to work on a server on which Tailscale is properly installed and set for the funnel as follows.

Forward access from the Tailscale network port 443 to the local port 11000 where `nextcloud-aio-apache` is running.
```bash
sudo tailscale funnel --bg --https=443 11000
```

Apply forward setting on your router and firewall.

## Useful snippets
### Delete existing docker containers and networks.

```bash
docker compose down;\
docker stop $(docker ps -aq);\
docker rm $(docker ps -aq);\
docker network rm nextcloud-aio
```
*To re-initiate this app you have to delete the data dir and volumes of `nextcloud-aio-mastercontainer` after the command above.