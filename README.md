# Nextcloud All-in-One with Tailscale 
This repository stores the optimal configuration for running [Nextcloud All-in-One](https://github.com/nextcloud/all-in-one) with Tailscale[Tailscale](https://github.com/tailscale/tailscale) on my home server.
Built for myself. If it helps you, even better.  
> Currently, only public-hostscale is actively maintained since I use it as my main driver.

- [Variants](#variants)
  - [local](#local)
  - [public](#public)
  - [public-hostscale (preferred)](#public-hostscale-preferred)
- [Tips](#tips)
  - [Delete existing docker containers and networks.](#delete-existing-docker-containers-and-networks)
  - [Boot configuration](#boot-configuration)
  - [Maintenance](#maintenance)

## Variants
For various use cases, three variants are provided. Each variant's name aligns with its corresponding directory in this repository.

### local
This runs only within your Tailscale network.
[Found the seed of this solution here](https://github.com/nextcloud/all-in-one/discussions/5439#discussioncomment-11935630)

### public
This runs on the internet so that you can share with anybody without inviting them on your Tailscale network.
[Found the seed of this solution here](https://github.com/nextcloud/all-in-one/discussions/5439#discussioncomment-11696448)

### public-hostscale (preferred)
This solution is intended for a server with Tailscale pre-installed and its funnel option activated. Essential setup steps include:

* **HTTPS Enablement:** Configure Tailscale certificates to facilitate secure HTTPS connections.
* **Tailscale Funneling:** Direct incoming requests from the Tailscale network's port 443 to the local `nextcloud-aio-apache` instance on port 11000.
    ```bash
    sudo tailscale funnel --bg --https=443 11000
    ```
* **Router Port Forwarding:** Set a static IP for the server on your router, then forward the Nextcloud Talk port.
* **Firewall Adjustment:** Confirm your firewall settings are open for all required network traffic.
* **A insecure connection must be established when you are using the AIO containers manager.
    ```bash
    sudo tailscale serve --https=8888 https+insecure://127.0.0.1:8080
    ```

## Tips
### Delete existing docker containers and networks.

```bash
docker compose down;\
docker stop $(docker ps -aq);\
docker rm $(docker ps -aq);\
docker network rm nextcloud-aio
```
*To initiate this app you have to delete the data dir and volumes of `nextcloud-aio-mastercontainer` after the command above.

### Boot configuration
- [Create systemd service](https://linuxhandbook.com/create-systemd-services/) to turn off built-in displays at the boot.
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

### Maintenance
- Update and upgrade apt packages
- Login to Nextcloud as admin
- Go to the setting and open Nextcloud AIO Interface
this are  good.
