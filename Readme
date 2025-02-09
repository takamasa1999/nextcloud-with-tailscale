# Nextcloud All-in-One with Tailscale 
This repository is made to save the settings that I tried to run [Nextcloud All-in-One](https://github.com/nextcloud/all-in-one) with domain names provided by [Tailscale](https://github.com/tailscale/tailscale) on my home server.  
This project is primarily made for my self to save the best settings on my server environment. If this accidentally helps you somehow, that's more than a pleasure.


## local
Runs only within your Tailscale network.
[Found the seed of this solution here](https://github.com/nextcloud/all-in-one/discussions/5439#discussioncomment-11935630)

## public
Runs on internet so that you can share with anybody without inviting them to your Tailscale network.
[Found the seed of this solution here](https://github.com/nextcloud/all-in-one/discussions/5439#discussioncomment-11696448)

## Useful snippets
### Delete existing docker containers and networks.

```bash
docker compose down;\
docker stop $(docker ps -aq);\
docker rm $(docker ps -aq);\
docker network rm nextcloud-aio
```