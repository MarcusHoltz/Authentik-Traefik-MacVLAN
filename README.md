# Setting Up Bridged Docker MacVLAN Network for Authentik to Access Traefik


I want to keep Traefik on it's own IP, but now I need [host to container communication](https://blog.holtzweb.com/posts/docker-macvlan-for-traefik-analytic-dashboards/#step-4-enable-host-to-container-communication) to allow Authentik to pass tokens.

The OAuth flow requires bidirectional communication between services that may be on different networks (Traefik), that OAuth token exchange requires multiple HTTP requests between the services and Authentik.


* * *

To facilitate this: 

- Traefik has it's own IP address on host network via a MacVLAN.

- The host's IP is used for all other containers, standard to how docker normally operates.


* * *

For the full guide that goes along with this repo, visit https://blog.holtzweb.com.



* * *

## Demo 

![Docker MacVLAN Bridge with Traefik Demo of Script Running](https://raw.githubusercontent.com/MarcusHoltz/marcusholtz.github.io/refs/heads/main/assets/img/posts/traefik-macvlan-bridge--host-to-container-communication.gif)





* * *

## Install Script

The only requirement is Debian 12. 

The rest of the script covers all materials needed to have:

- MacVLAN on the host interface 

- Docker MacVLAN for Traefik

- MacVLAN bridge for Authentik

- Traefik's access logs with original source headers

- Analytic dashboard with Promtail/Loki/Grafana


* * *

### System Requirements

Make sure this is a Debian 12 system you're working with. I dont think I have mentioned this at all?


#### Debian 12 only

Oh look there, yes, `Debian 12`. You may find a path to do this with other methods, but out of the box LXC or VM - we're going with **Debian**.


* * *

### Script Features

- **Automated Traefik + MacVLAN Setup:** Configures Traefik with a MacVLAN Docker network for simplified reverse proxy.

- **Docker Installation** (Optional): Installs Docker if not already present on the system.

- **Automatic Network Information:** Detects and stores host network details (IP, gateway, subnet).

- **Systemd-Networkd Integration:** Generates and applies systemd network configuration files for MacVLAN bridge support.

- **ifupdown networking disable**: Disables ifupdown2 networking if systemd-networkd is enabled.

- **Context-Aware Configuration**: Adapts its configuration steps based on whether it is running before or after a system reboot, ensuring proper setup in either scenario.

- **LXC virtualization check**: Will alter the script depending on the virtual environment.

- **Dynamic IP Assignment:** Automatically assigns an IP address to the Traefik MacVLAN for access.

- **Docker Network Management:** Creates `proxy2traefik` for Docker container communication and `traefik2host` for Traefik's personal MacVLAN, along with isolated networks for all databases.

- **Update .env File**: Save all collected variables and Authentik secret key.

- **Configuration File Management:** Verifies, downloads, and sets up necessary configuration files for Traefik, Promtail, and Grafana.

- **Docker Compose Deployment:** Deploys the entire Authentik and Traefik stack using Docker Compose.

- **Informational Output**: Provides post-configuration instructions, including DNS record setup and application access URLs, to guide the user.



* * *


