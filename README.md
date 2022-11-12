# mulle-dockerize

#### 🔌 Collection of Dockerized Commands

Don't want to mess up your system with `npm` or `jekyll` and its ilk ? No
problem run them in a docker.

This project supplies dockerized "yo", "node", "npm", "vsce" and "jekyll"
commands. The docker image for the command will be built the first time a
command is executed. The produced files will have the permissions of the logged
in user and not some pseudo docker user or even ... root.

> Because of unix group/user permissions, the dockerized commands will only
> work for the installing user. So everything shouldl be installed local to the
> users home.


### Bonus Action in 0.2.0

So you know, that a mulle-dockerized container already contains the command,
that you want to dockerize ? No need to create a new container! Just symlink
to the symlink.

In this example we just use `ls` from the ubuntu base package (which the node
container is based on) for demonstration purposes:

```sh
ln -s ~/bin/node ~/bin/ls
```


## Install

Run `./bin/installer` from the project root to install. For development
purposes it's likely better to use `./bin/symlinker` to install. Or you can
do it all manually:


#### Jekyll

``` sh
PREFIX="${HOME:-~}"

mkdir   -p "${PREFIX}/bin"
mkdir   -p "${PREFIX}/share/mulle-dockerize/jekyll"

install -v share/jekyll/Dockerfile "${PREFIX}/share/mulle-dockerize/jekyll/Dockerfile"
install -v share/jekyll/Gemfile    "${PREFIX}/share/mulle-dockerize/jekyll/Gemfile"

install -v mulle-dockerize "${PREFIX}/bin/mulle-dockerize"
install -v jekyll "${PREFIX}/bin/jekyll"
```

Now you can just say `jekyll serve -d /tmp/whatevs`. If you still get package
retrieval, while starting the command, remove `Gemfile.lock` and rerun. If
package retrieval persists, then copy your Gemfile to
`~/share/mulle-dockerize/jekyll/Gemfile` and let the container be
[rebuilt](#rebuild-a-command).


#### Node and NPM

``` sh
PREFIX="${HOME:-~}"

mkdir -p "${PREFIX}/bin"
mkdir -p "${PREFIX}/share/mulle-dockerize/node"
mkdir -p "${PREFIX}/share/mulle-dockerize/npm"

install -v share/node/Dockerfile   "${PREFIX}/share/mulle-dockerize/node/Dockerfile"
install -v share/npm/Dockerfile    "${PREFIX}/share/mulle-dockerize/npm/Dockerfile"

install -v mulle-dockerize "${PREFIX}/bin/mulle-dockerize"

ln -v -s -f "${PREFIX}/bin/mulle-dockerize" "${PREFIX}/bin/node"
ln -v -s -f "${PREFIX}/bin/mulle-dockerize" "${PREFIX}/bin/npm"
```

### VSCE

``` sh
PREFIX="${HOME:-~}"

mkdir -p "${PREFIX}/bin"
mkdir -p "${PREFIX}/share/mulle-dockerize/yo"
mkdir -p "${PREFIX}/share/mulle-dockerize/vsce"

install -v share/yo/Dockerfile     "${PREFIX}/share/mulle-dockerize/yo/Dockerfile"
install -v share/vsce/Dockerfile   "${PREFIX}/share/mulle-dockerize/vsce/Dockerfile"

install -v mulle-dockerize "${PREFIX}/bin/mulle-dockerize"

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

## Tips & Tricks

#### Electron on Linux

Setup for electron apps: You probably need to set `xhost +local:root` and then
invoke your app with:

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

This invocation follows [this example](https://medium.com/ingeniouslysimple/building-an-electron-app-from-scratch-part-1-a1d9012c146a)).

#### Rebuild a command

Get rid of a the docker containers for a certain image and the image itself:

``` sh
image="${USERNAME}-jekyll"
docker rm $(docker ps -a -q --filter "ancestor=${image}")
docker rmi "${image}"
```

Now just rerun the command and the container will be rebuilt.

## Author

[Nat!](//www.mulle-kybernetik.com/weblog) for
[Mulle kybernetiK](//www.mulle-kybernetik.com)
