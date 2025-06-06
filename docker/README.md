# Docker

This installs `docker` and `docker-compose`:

```sh
sudo sh -c 'groupadd docker;\
gpasswd -a $USER docker;\
zypper -n install docker docker-compose;\
systemctl enable docker;\
systemctl start docker'
```

The group changes require a restart (or a new shell with `newgrp docker`, this will be bash though).

Subsequently demonstrate it all works with: `docker run hello-world`
