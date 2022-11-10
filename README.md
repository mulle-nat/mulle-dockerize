# mulle-dockerize

#### 🔌 JavaScript Dockerized Commands

Don't want to mess up your system with `npm` and its ilk ? No problem run
them in a docker.


Supplies dockerized "yo", "node", "npm", "vsce" commands. The docker image for
the command will be built the first time a command is executed.

> Because of unix group/user permissions, the dockerized commands will only
> work for the installing user. So everything will be installed local to the
> users home.

#### node container

Setup for electron apps. You probably need to set `xhost +local:root` and then
invoke your app with (following [this example](https://medium.com/ingeniouslysimple/building-an-electron-app-from-scratch-part-1-a1d9012c146a)):

``` sh
ELECTRONFLAGS="${ELECTRONFLAGS:--no-sandbox}"

DOCKERRUNFLAGS="${DOCKERRUNFLAGS} \
--security-opt apparmor=unconfined \
--env DBUS_SESSION_BUS_ADDRESS \
--env=DISPLAY \
--env=QT_X11_NO_MITSHM=1 \
--volume /sys/fs/cgroup:/sys/fs/cgroup:ro \
--volume=/run/dbus:/run/dbus:rw \
--volume=/run/user:/run/user:rw \
--volume=/tmp/.X11-unix:/tmp/.X11-unix:rw" \
   ./node_modules/.bin/electron ${ELECTRONFLAGS} ./src/electron/index.js "$@"
```


### Bonus Action in 0.1.0

So you know you a mulle-dockerized container already contains the command, that
you want to dockerize ? No need to create a new container! Just symlink to
the symlink.

In this example we just use `ls` from the ubuntu base package for demonstration
purposes:

```sh
ln -s ~/bin/node ~/bin/ls
```


## Install

Run `./bin/installer` or

``` sh
PREFIX="${HOME}"

mkdir -p "${PREFIX}/share/mulle-dockerize/npm"
mkdir -p "${PREFIX}/share/mulle-dockerize/yo"
mkdir -p "${PREFIX}/share/mulle-dockerize/vsce"
mkdir -p "${PREFIX}/share/mulle-dockerize/node"
mkdir -p "${PREFIX}/bin"

install -v node/Dockerfile "${PREFIX}/share/mulle-dockerize/vsce/Dockerfile"
install -v npm/Dockerfile "${PREFIX}/share/mulle-dockerize/npm/Dockerfile"
install -v yo/Dockerfile "${PREFIX}/share/mulle-dockerize/yo/Dockerfile"
install -v vsce/Dockerfile "${PREFIX}/share/mulle-dockerize/vsce/Dockerfile"

install -v mulle-dockerize "${PREFIX}/bin/mulle-dockerize"

ln -v -s -f "${PREFIX}/bin/mulle-dockerize" "${PREFIX}/bin/npm"
ln -v -s -f "${PREFIX}/bin/mulle-dockerize" "${PREFIX}/bin/node"
ln -v -s -f "${PREFIX}/bin/mulle-dockerize" "${PREFIX}/bin/yo"
ln -v -s -f "${PREFIX}/bin/mulle-dockerize" "${PREFIX}/bin/vsce"
```

Maybe add this to your `.bash_profile`:

``` sh
alias yo="yo --no-insight"
```

### Ubuntu

Don't mess around. Put your user in the "docker" group and don't `sudo`
all the time.

[How can I use docker without sudo?](https://askubuntu.com/questions/477551/how-can-i-use-docker-without-sudo)

``` sh
sudo groupadd docker
sudo gpasswd -a $USER docker
newgrp docker
```
