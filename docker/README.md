# Docker

This installs `docker` and `docker-compose`:

```sh
sudo groupadd docker
sudo gpasswd -a $USER docker
sudo zypper install docker docker-compose
sudo systemctl enable docker
sudo systemctl start docker
```

The group changes require a restart (or a new shell with `newgrp docker`, this will be bash though).

Subsequently demonstrate it all works with: `docker run hello-world`
