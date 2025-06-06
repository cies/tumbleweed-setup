# Docker

This installs `docker` and `docker-compose`:

```sh
sudo sh -c 'groupadd docker;\
zypper -n install docker docker-compose;\
systemctl enable docker;\
systemctl start docker'
```

And:

```sh
sudo gpasswd -a $USER docker
```

The group changes require a restart (or a new shell with `newgrp docker`, this will be bash though).

Subsequently demonstrate it all works with: `docker run hello-world`
