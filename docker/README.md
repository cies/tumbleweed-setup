# Docker

This installs `docker` and `docker-compose`:

```sh
sudo groupadd docker
sudo gpasswd -a $USER docker
newgrp docker
sudo zypper install docker docker-compose
sudo systemctl enable docker
sudo systemctl start docker
```

Subsequently demonstrate it all works with: `docker run hello-world`
