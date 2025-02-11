# Nextcloud All-in-One with Tailscale 
This repository is made to save the best setting that I run [Nextcloud All-in-One](https://github.com/nextcloud/all-in-one) with  [Tailscale](https://github.com/tailscale/tailscale) on my home server.  
This project is primarily made for myself. If this accidentally helps you somehow, that's more than a pleasure.


## local
This runs only within your Tailscale network.
[Found the seed of this solution here](https://github.com/nextcloud/all-in-one/discussions/5439#discussioncomment-11935630)

## public
This runs on the internet so that you can share with anybody without inviting them on your Tailscale network.
[Found the seed of this solution here](https://github.com/nextcloud/all-in-one/discussions/5439#discussioncomment-11696448)

## public-hostscale (preferred)
This is intended to work on a server on which Tailscale is properly installed and set for the funnel option as follows.

- Set up cert for Tailscale so that https connection becomes available
- Forward access from the tailnet port 443 to the local port 11000 where `nextcloud-aio-apache` is running.
    ```bash
    sudo tailscale funnel --bg --https=443 11000
    ```
- On your router supply static IP for the server then set port forwarding to the one which Nextcloud Talk uses
- Check your firewall setting is properly set to accept all necessary traffics


## Tips
### Delete existing docker containers and networks.

```bash
docker compose down;\
docker stop $(docker ps -aq);\
docker rm $(docker ps -aq);\
docker network rm nextcloud-aio
```
*To initiate this app you have to delete the data dir and volumes of `nextcloud-aio-mastercontainer` after the command above.

### Set up boot options
- [Create systemd service](https://linuxhandbook.com/create-systemd-services/) to turn off displays at the boot.
  In my case I registered the command below to the service.
  ```bash
  sudo vbetool dpms off
  ```
- [Set fstab](https://www.howtogeek.com/38125/htg-explains-what-is-the-linux-fstab-and-how-does-it-work/) to mount your external drive at the boot
  Used the command below to check the UUID of the target drive.
  ```bash
  lsblk -o KNAME,TYPE,SIZE,MODEL,UUID
  ```
  Don't forget to execute below before rebooting your computer to check whether the new setting is working properly
    ```bash
    sudo mount -a
    ```