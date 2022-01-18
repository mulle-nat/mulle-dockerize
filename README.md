# mulle-dockerize

#### 🔌 Visual Studio Code Extension Environment


Supplies dockerized "yo", "npm", "vsce" commands. The docker image for the
command will be built the first time a command is executed.

> Because of unix group/user permissions, the dockerized commands will only
> work for the installing user. So everything will be installed local to the
> users home.

## Install

Run `./bin/installer` or

```
PREFIX="${HOME}"

mkdir -p "${PREFIX}/share/mulle-dockerize/npm"
mkdir -p "${PREFIX}/share/mulle-dockerize/yo"
mkdir -p "${PREFIX}/share/mulle-dockerize/vsce"
mkdir -p "${PREFIX}/bin"

install -v npm/Dockerfile "${PREFIX}/share/mulle-dockerize/npm/Dockerfile"
install -v yo/Dockerfile "${PREFIX}/share/mulle-dockerize/yo/Dockerfile"
install -v vsce/Dockerfile "${PREFIX}/share/mulle-dockerize/vsce/Dockerfile"
install -v mulle-dockerize "${PREFIX}/bin/mulle-dockerize"
ln -v -s -f "${PREFIX}/bin/mulle-dockerize" "${PREFIX}/bin/npm"
ln -v -s -f "${PREFIX}/bin/mulle-dockerize" "${PREFIX}/bin/yo"
ln -v -s -f "${PREFIX}/bin/mulle-dockerize" "${PREFIX}/bin/vsce"
```

Maybe add this to your `.bash_profile`:

```
alias yo="yo --no-insight"
```

### Ubuntu

Don't mess around. Put your user in the "docker" group and don't `sudo`
all the time.

[How can I use docker without sudo?](https://askubuntu.com/questions/477551/how-can-i-use-docker-without-sudo)

```
sudo groupadd docker
sudo gpasswd -a $USER docker
newgrp docker
```
