# mulle-dockerize

#### 🔌 Visual Studio Code Extension Environment


Supplies a dockerized "yo" and a dockerized "npm" command. The docker images
will be built the first time the commands are executed.

> Because of unix group/user permissions, the dockerized commands will only
> work for the installing user. So everything will be installed local to the
> users home.

## Install

```
PREFIX="${HOME}"
install -D mulle-dockerize "${PREFIX}/bin/mulle-dockerize"
install -D npm/Dockerfile "${PREFIX}/share/mulle-dockerize/npm/Dockerfile"
install -D yo/Dockerfile "${PREFIX}/share/mulle-dockerize/yo/Dockerfile"
ln -s -f "${PREFIX}/bin/mulle-dockerize" "${PREFIX}/bin/npm"
ln -s -f "${PREFIX}/bin/mulle-dockerize" "${PREFIX}/bin/yo"
```

Maybe add this to your `.bash_profile`:

```
alias yo="yo --no-insight"
```

### Ubuntu

Don't mess around, put your user in the "docker" group and don't `sudo`
all the time.

[How can I use docker without sudo?](https://askubuntu.com/questions/477551/how-can-i-use-docker-without-sudo)

```
sudo groupadd docker
sudo gpasswd -a $USER docker
newgrp docker
```
